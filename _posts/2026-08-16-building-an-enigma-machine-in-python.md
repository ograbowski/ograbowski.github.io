---
layout: post
title: "Building an Enigma machine in Python"
---

One evening I sat down to write a simulator of the Enigma I, the cipher
machine the German military used in World War II. Pure Python, no libraries.
It ended up being about 150 lines of code, and honestly, I learned more about
how the machine works from those 150 lines than from everything I had read
about it before. The code is [on GitHub](https://github.com/ograbowski/Enigma).

## Why Enigma?

Partly because the story hits close to home. Before Bletchley Park and before
Turing, there were three Polish mathematicians: Marian Rejewski, Jerzy Różycki
and Henryk Zygalski. In 1932 Rejewski figured out the internal wiring of the
Enigma rotors using permutation theory. He never saw the inside of the
machine. The Poles then built machines to attack it (the cyclometer, and
something wonderfully named the "cryptologic bomb"), and in the summer of
1939, weeks before the war started, they handed everything over to the French
and the British, working replicas included. Bletchley Park didn't start from
zero. It started from Rejewski.

The other reason is simpler: Enigma is a perfect weekend project. The machine
is just a chain of letter substitutions, but there are enough moving parts
that you can't fake it. Either your simulator produces exactly what a real
Enigma I would, or it doesn't, and you get to find out why.

## How the machine works, in one paragraph

You press a key. The rotors step first, then the electrical signal runs
through the plugboard, through three rotors right to left, bounces off the
reflector, comes back through the rotors left to right, and through the
plugboard again. A lamp lights up: that's your ciphertext letter. The
reflector makes the whole thing symmetric, so encryption is its own inverse.
Type the ciphertext into a machine with the same settings and the plaintext
comes back out. It also means a letter can never encrypt to itself, which
turned out to be the machine's fatal flaw. The codebreakers loved it.

I used the real specs: the historical wiring of rotors I, II and III with
their turnover notches, and the UKW-B reflector.

## The part everyone gets wrong

The rotors work roughly like a car odometer. The right rotor steps on every
keypress, and when it passes its notch, it kicks the middle one forward, and
so on. Roughly. Because there's a mechanical quirk in the stepping levers:
when the middle rotor sits on its own notch, it pushes the left rotor forward
and steps itself at the same time. This is the famous double-stepping anomaly,
and it's the thing most toy simulators get wrong.

Mine included. My first commit checked the two notches with two independent
ifs, so the middle rotor never stepped itself. Here's the fixed version:

```python
def step_rotors(self):
    left, middle, right = self.rotors
    middle_at_notch = middle.at_notch()
    right_at_notch = right.at_notch()

    if middle_at_notch:
        left.step()
        middle.step()   # the double step
    elif right_at_notch:
        middle.step()

    right.step()
```

There's one more trap hiding in `at_notch()`. The turnover depends on the
letter visible in the rotor window, and that letter does not care about the
ring setting. So the check has to look at the raw position, not the
ring-adjusted one. Ask me how I know.

## Ring settings without a headache

The ring setting (Ringstellung) rotates the wiring relative to the rotor body.
You could model it as a separate component, but it turns out to be just a
symmetric shift around the wiring lookup:

```python
def forward(self, value):
    shifted = (value + self.position - self.ring) % 26
    wired = self.forward_map[shifted]
    return (wired - self.position + self.ring) % 26
```

The inverse table for the return path gets built once, when the rotor is
created. The plugboard is even simpler: an array that starts as the identity
permutation, with two entries swapped for each cable. It's its own inverse, so
the same array works in both directions.

## Does it actually work?

The demo reads a line from stdin, encrypts it, then feeds the result to a
second machine with the same daily key:

```
$ echo "ATTACK AT DAWN" | python3 enigma.py
Input: ATTACK AT DAWN
Encrypted text: SRDYJT WD QLZJ
Decrypted text: ATTACK AT DAWN
```

Round trip works. And notice that no letter maps to itself, just like the real
thing. The settings play the role of the Kenngruppenbuch, the daily key sheet:
rotor order, starting positions, ring settings, and ten plugboard pairs, which
was the standard wartime count.

## What's missing

Only rotors I to III and reflector B. No four-rotor naval M4, no Uhr, and the
daily key is hardcoded instead of coming from the command line. All of that
would be fun to add on some rainy afternoon. But the follow-up I actually want
to do is the one Rejewski did: don't build the machine, break it.
