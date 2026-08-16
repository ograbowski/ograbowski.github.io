---
layout: post
title: "Building an Enigma machine in Python"
---

A while ago I spent an evening building a simulator of the Enigma I — the cipher
machine used by the German military in World War II — in pure Python, no
dependencies. The whole thing came out to about 150 lines and it taught me more
about the machine than any article I had read about it. The code is
[on GitHub](https://github.com/ograbowski/Enigma).

## Why Enigma?

Partly because the story is personal. Before Bletchley Park, before Turing,
there were three Polish mathematicians — Marian Rejewski, Jerzy Różycki and
Henryk Zygalski — working at the Polish Cipher Bureau. In 1932 Rejewski
reconstructed the internal wiring of the Enigma rotors using permutation
theory, without ever seeing the inside of the machine. The Poles built the
cyclometer and the "cryptologic bomb" to attack it, and in the summer of 1939,
weeks before the war broke out, they handed all of it — including working
replicas — to the French and the British. Bletchley Park didn't start from
zero; it started from Rejewski.

The other reason: Enigma is a perfect weekend-sized project. The machine is
just a chain of substitutions — plugboard, three rotors, reflector, and back —
but it has enough moving parts that you can't fake your way through it. Either
your simulator produces the exact same ciphertext as a real Enigma I, or it
doesn't.

## The machine in one paragraph

Every keypress first steps the rotors, then sends a signal through the
plugboard, through three rotors right-to-left, bounces off the reflector, comes
back through the rotors left-to-right, and through the plugboard again. Because
the reflector makes the whole transform symmetric, encryption is its own
inverse: type the ciphertext into a machine with the same settings and you get
the plaintext back. It also means a letter can never encrypt to itself — the
famous flaw that gave the codebreakers their cribs.

I used the real Enigma I specs: the historical wirings of rotors I–III with
their turnover notches, and the UKW-B reflector.

## The part everyone gets wrong: double-stepping

The rotors are basically an odometer — the right rotor steps on every
keypress, and when it passes its notch it advances the middle one, and so on.
Except it's *not* quite an odometer, because of a mechanical quirk of the
stepping levers: when the middle rotor sits on its own notch, it advances the
left rotor **and steps itself** at the same time. This is the notorious
double-stepping anomaly, and it's the detail most toy simulators get wrong.

I got it wrong too. My first commit checked the two notches independently, so
the middle rotor never stepped itself. The fix:

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

One subtlety hiding in `at_notch()`: the turnover is driven by the letter
visible in the rotor window, which does **not** depend on the ring setting —
so the check has to compare the raw position, not the ring-adjusted one.

## Ring settings without a headache

The Ringstellung (ring setting) rotates the wiring relative to the rotor body.
Instead of modelling it as a separate component, it folds neatly into a
symmetric shift around the wiring lookup:

```python
def forward(self, value):
    shifted = (value + self.position - self.ring) % 26
    wired = self.forward_map[shifted]
    return (wired - self.position + self.ring) % 26
```

The inverse table for the return path is precomputed once when the rotor is
built. The plugboard is even simpler: a permutation array that starts as the
identity and swaps two entries per cable — self-inverse by construction, so
the same array works on the way in and on the way out.

## Proof it works

The demo encrypts a line from stdin, then feeds the result to a second machine
instance with the same daily key:

```
$ echo "ATTACK AT DAWN" | python3 enigma.py
Input: ATTACK AT DAWN
Encrypted text: SRDYJT WD QLZJ
Decrypted text: ATTACK AT DAWN
```

Notice that no letter maps to itself — the machine's fatal weakness, working
as historically intended. The settings dict plays the role of the
Kenngruppenbuch, the daily key sheet: rotor order (Walzenlage), starting
positions (Grundstellung), ring settings (Ringstellung), and ten plugboard
pairs — the standard wartime count.

## What's not there

Only rotors I–III and reflector B, no four-rotor naval M4, no Uhr attachment,
and the daily key is hardcoded rather than taken from the command line. All
fair game for a rainy afternoon. The more interesting follow-up, though, would
be to go where Rejewski went: don't build the machine — break it.
