The Synthesized Lakh (Slakh) Dataset is a new dataset for audio source separation that is synthesized from the [Lakh MIDI Dataset v0.1](https://colinraffel.com/projects/lmd/)[1] using professional grade sample-based virtual instruments. This first release of Slakh, called Slakh2100, contains 2100 automatically mixed tracks and accompanying MIDI files synthesized using the [Native Instruments' Komplete Complete 12 Ultimite](https://www.native-instruments.com/en/products/komplete/bundles/komplete-12-ultimate/) sample pack with their Kontakt sampling engine.


## Table of Contents

1. [Get Slakh2100](#download)
2. [Comparison to Other Datasets](#comparison)
3. [License and Attribution](#license)
4. [Construction of Slakh](#construction)
    1. [Selection from the Lakh MIDI Dataset](#selection)
    2. [Rendering and Mixing](#rendering)
    3. [Flakh2100](#flakh)
5. [Analysis](#analysis)
6. [Examples](#examples)
7. [Benchmarks](#benchmarks)
8. [Citations](#citations)
 

## Get Slakh2100 <a name="download"></a>

Download links coming soon!

## License and Attribution <a name="license"></a>
  
If you use Slakh2100 or generate data using the same method we ask that you cite it using the following bibtex entry:

<BibTex entry here>

## Comparison to Other Datasets <a name="comparison"></a>

Here is a comparison between Slakh2100 and the most popular datasets used for audio source separation. Note that MUSDB18 combines data from DSD100 and MedleyDB, which is denoted with an asterisk (\*) below.

| Dataset       | # Songs  | Size (h) | # Tracks | # Instrument Categories |
|---------------|----------|----------|----------|-------------------------|
| iKala [2]     | 306      | 2        | 2        | 2                       |
| MIR-1k [3]    | 110      | 2.25     | 2        | 2                       |
| DSD100* [4]   | 100      | 7        | 4        | 4                       |
| MedleyDB* [5] | 122      | 7.2      | 1-26     | 82                      |
| MUSDB18* [6]  | 150      | 10       | 4        | 4                       |
| **Slakh2100** | **2100** | **145**  | **4-48** | **34**                  |


Here is how Slakh2100 compares to the full Lakh MIDI Dataset (`lmd-full`) and to the subset of Lakh that has the four instrument categories as described in [Selection from the Lakh MIDI Dataset](#selection) (this is denoted as "LMD Subset" in the table below).

| Dataset               | # Songs  | Size (h) | # Tracks | # Instrument Categories |
|-----------------------|----------|----------|----------|-------------------------|
| LMD (subset)          | 20371    | 1793     | 4+       | 129                     |
| Lakh MIDI Dataset [1] | 176581   | 10521    | 1+       | 129                     |
| **Slakh2100**         | **2100** | **145**  | **4-48** | **34**                  |


## Construction of Slakh <a name="construction"></a>
### Selection from the Lakh MIDI Dataset <a name="selection"></a>

### Rendering and Mixing <a name="rendering"></a>

### Flakh <a name="flakh"></a>

## Anaylsis <a name="analysis"></a>

## Examples <a name="examples"></a>

## Benchmarks <a name="benchmarks"></a>


## Citations <a name="citations"></a>

[1] Colin Raffel. "Learning-Based Methods for Comparing Sequences, with Applications to Audio-to-MIDI Alignment and Matching". PhD Thesis, 2016.
