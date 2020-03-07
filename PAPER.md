# Permoire

The key is a set of four 4-bit permutations.
It contains log2((2<sup>8</sup>)!) × 4 = 178 bits of entropy,
but they are each encoded as 64 bits (256-bit total)
for convenience and to avoid side-channel attacks.

The main construct is a PRF with a 128 bit block size.

Below is a single round.
Each dot represents a 4-bit input or output;
digits identify one of the four permutations; and merging paths are XOR.

        ┌────────────────────────────────────────────────────────────────────────────┐
        │   ┌───────────────────────────────────────────────────────────────────────┐│
        │   │   ┌──────────────────────────────────────────────────────────────────┐││
        │   │   │   ┌─────────────────────────────────────────────────────────────┐│││
        │   │   │   │   ┌────────────────────────────────────────────────────────┐││││
        │   │   │   │   │   ┌───────────────────────────────────────────────────┐│││││
        │   │   │   │   │   │   ┌──────────────────────────────────────────────┐││││││
        │   │   │   │   │   │   │   ┌─────────────────────────────────────────┐│││││││
        │   │   │   │   │   │   │   │   ┌────────────────────────────────────┐││││││││
        │   │   │   │   │   │   │   │   │   ┌───────────────────────────────┐│││││││││
        │   │   │   │   │   │   │   │   │   │   ┌──────────────────────────┐││││││││││
        │   │   │   │   │   │   │   │   │   │   │   ┌─────────────────────┐│││││││││││
        │   │   │   │   │   │   │   │   │   │   │   │   ┌────────────────┐││││││││││││
    i   │   │   │   │   │   │   │   │   │   │   │   │   │   ┌───────────┐│││││││││││││
    n   │   │   │   │   │   │   │   │   │   │   │   │   │   │   ┌──────┐││││││││││││││
    p ┌─┤ ┌─┤ ┌─┤ ┌─┤ ┌─┤ ┌─┤ ┌─┤ ┌─┤ ┌─┤ ┌─┤ ┌─┤ ┌─┤ ┌─┤ ┌─┤ ┌─┤ ┌─┬─┐│││││││││││││││
    u ╹ ╹ ╹ ╹ ╹ ╹ ╹ ╹ ╹ ╹ ╹ ╹ ╹ ╹ ╹ ╹ ╹ ╹ ╹ ╹ ╹ ╹ ╹ ╹ ╹ ╹ ╹ ╹ ╹ ╹ ╹ ╹ ││││││││││││││││
    t 3 4 1 2 3 4 1 2 3 4 3 4 1 2 3 4 1 2 3 4 3 4 1 2 3 4 1 2 3 4 1 2 ││││││││││││││││
    ↳ . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . ││││││││││││││││
      1 2 3 4 1 2 3 4 1 2 3 4 1 2 3 4 1 2 3 4 1 2 3 4 1 2 3 4 1 2 3 4 ││││││││││││││││
      ╻ ╻ ╻ ╻ ╻ ╻ ╻ ╻ ╻ ╻ ╻ ╻ ╻ ╻ ╻ ╻ ╻ ╻ ╻ ╻ ╻ ╻ ╻ ╻ ╻ ╻ ╻ ╻ ╻ ╻ ╻ ╻ ││││││││││││││││
      ├─┘ ├─┘ ├─┘ ├─┘ ├─┘ ├─┘ ├─┘ ├─┘ ├─┘ ├─┘ ├─┘ ├─┘ ├─┘ ├─┘ ├─┘ ├─┘ ││││││││││││││││
      │ ┌─┘ ┌─┘ ┌─┘ ┌─┘ ┌─┘ ┌─┘ ┌─┘ ┌─┘ ┌─┘ ┌─┘ ┌─┘ ┌─┘ ┌─┘ ┌─┘ ┌─┘   ││││││││││││││││
      │ │ ┌─┘ ┌─┘ ┌─┘ ┌─┘ ┌─┘ ┌─┘ ┌─┘ ┌─┘ ┌─┘ ┌─┘ ┌─┘ ┌─┘ ┌─┘ ┌─┘     ││││││││││││││││
      │ │ │ ┌─┘ ┌─┘ ┌─┘ ┌─┘ ┌─┘ ┌─┘ ┌─┘ ┌─┘ ┌─┘ ┌─┘ ┌─┘ ┌─┘ ┌─┘       ││││││││││││││││
      │ │ │ │ ┌─┘ ┌─┘ ┌─┘ ┌─┘ ┌─┘ ┌─┘ ┌─┘ ┌─┘ ┌─┘ ┌─┘ ┌─┘ ┌─┘         ││││││││││││││││
      │ │ │ │ │ ┌─┘ ┌─┘ ┌─┘ ┌─┘ ┌─┘ ┌─┘ ┌─┘ ┌─┘ ┌─┘ ┌─┘ ┌─┘           ││││││││││││││││
      │ │ │ │ │ │ ┌─┘ ┌─┘ ┌─┘ ┌─┘ ┌─┘ ┌─┘ ┌─┘ ┌─┘ ┌─┘ ┌─┘             ││││││││││││││││
      │ │ │ │ │ │ │ ┌─┘ ┌─┘ ┌─┘ ┌─┘ ┌─┘ ┌─┘ ┌─┘ ┌─┘ ┌─┘               ││││││││││││││││
      │ │ │ │ │ │ │ │ ┌─┘ ┌─┘ ┌─┘ ┌─┘ ┌─┘ ┌─┘ ┌─┘ ┌─┘                 ││││││││││││││││
      │ │ │ │ │ │ │ │ │ ┌─┘ ┌─┘ ┌─┘ ┌─┘ ┌─┘ ┌─┘ ┌─┘                   ││││││││││││││││
      │ │ │ │ │ │ │ │ │ │ ┌─┘ ┌─┘ ┌─┘ ┌─┘ ┌─┘ ┌─┘                     ││││││││││││││││
      │ │ │ │ │ │ │ │ │ │ │ ┌─┘ ┌─┘ ┌─┘ ┌─┘ ┌─┘                       ││││││││││││││││
      │ │ │ │ │ │ │ │ │ │ │ │ ┌─┘ ┌─┘ ┌─┘ ┌─┘                         ││││││││││││││││
      │ │ │ │ │ │ │ │ │ │ │ │ │ ┌─┘ ┌─┘ ┌─┘                           ││││││││││││││││
      │ │ │ │ │ │ │ │ │ │ │ │ │ │ ┌─┘ ┌─┘                             ││││││││││││││││
      │ │ │ │ │ │ │ │ │ │ │ │ │ │ │ ┌─┘                               ││││││││││││││││
      ╽ ╽ ╽ ╽ ╽ ╽ ╽ ╽ ╽ ╽ ╽ ╽ ╽ ╽ ╽ ╽                                 ││││││││││││││││
      . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . ││││││││││││││││
      output                          ╻ ╻ ╻ ╻ ╻ ╻ ╻ ╻ ╻ ╻ ╻ ╻ ╻ ╻ ╻ ╻ ││││││││││││││││
                                      │ │ │ │ │ │ │ │ │ │ │ │ │ │ │ └─┘│││││││││││││││
                                      │ │ │ │ │ │ │ │ │ │ │ │ │ │ └────┘││││││││││││││
                                      │ │ │ │ │ │ │ │ │ │ │ │ │ └───────┘│││││││││││││
                                      │ │ │ │ │ │ │ │ │ │ │ │ └──────────┘││││││││││││
                                      │ │ │ │ │ │ │ │ │ │ │ └─────────────┘│││││││││││
                                      │ │ │ │ │ │ │ │ │ │ └────────────────┘││││││││││
                                      │ │ │ │ │ │ │ │ │ └───────────────────┘│││││││││
                                      │ │ │ │ │ │ │ │ └──────────────────────┘││││││││
                                      │ │ │ │ │ │ │ └─────────────────────────┘│││││││
                                      │ │ │ │ │ │ └────────────────────────────┘││││││
                                      │ │ │ │ │ └───────────────────────────────┘│││││
                                      │ │ │ │ └──────────────────────────────────┘││││
                                      │ │ │ └─────────────────────────────────────┘│││
                                      │ │ └────────────────────────────────────────┘││
                                      │ └───────────────────────────────────────────┘│
                                      └──────────────────────────────────────────────┘

PERMOIRE applies five rounds.

Note that the paths recursively form a binary tree from any given output dot.

## Recommended block mode

We recommend using it in counter mode:
for each message, pick a 128-bit initialization vector (IV).

Then, to encrypt a plaintext,
first cut it into 128-bit blocks.
For each plaintextBlock,
compute ciphertextBlock = PERMOIRE(IV + block number) XOR plaintextBlock.

Each message can encrypt up to 2<sup>64</sup> × 128 bits = 256 EiB.

## Diffusion

With exactly five rounds, PERMOIRE achieves provably perfect diffusion:
any given input bit flip changes every output bit with probability ½.

Number of bits affected in each 4-bit slice by the first input bit:

             10000000000000000000000000000000
    Round 1: 40000000000000004000000000000000
    Round 2: 40000000400000004000000040000000
    Round 3: 40004000400040004000400040004000
    Round 4: 40404040404040404040404040404040
    Round 5: 44444444444444444444444444444444

That provides a rare situation for a cipher,
wherein cryptanalysis is made extremely hard,
similar to a climber having to scale a flawlessly flat glass wall.

## Related-key attack

Confusion of the key is not perfect.

Indeed, a one-(entropy)-bit flip in the key can represent a single swap in one of the four permutations.
An output bit is affected by any given permutation at least 15 times
(once on round 2, twice on round 3, four times on round 4, eight on round 5).
For each case, one of the two 4-bit numbers that are swapped (out of 16)
can match the input with probability 2÷16.

Thus, the probability that a one-bit flip on the key affects a given output bit,
is 1 - (1 - 2÷16)<sup>15</sup> ≅ 87%.

This is not an issue assuming the key is truly random, as is commonly expected.

It is possible, for defense in depth, for each message,
to derive a message key from PERMOIRE(PERMOIRE(key\[0] XOR IV, key) XOR key\[1], key).

## Side-channel

A simple solution for key generation is to produce a random ≥178-bit number,
and to decode it as a Lehmer code.
That requires executing divisions which are [not guaranteed to be constant-time][Const].

TODO for constant-time solution involving Fisher-Yates shuffle with rejection sampling input.

[Const]: https://www.bearssl.org/constanttime.html

## Performance

We expect very good SIMD performance, from operating directly on bytes.
For example, with NEON, PERMOIRE can complete 128÷8 = 16 blocks in parallel.
For comparison, ChaCha20 can only do 128÷32 = 4.
