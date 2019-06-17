The Synthesized Lakh (Slakh) Dataset is a new dataset for audio source separation that is synthesized from the [Lakh MIDI Dataset v0.1](https://colinraffel.com/projects/lmd/)[1] using professional grade sample-based virtual instruments. This first release of Slakh, called **Slakh2100**, contains 2100 automatically mixed tracks and accompanying MIDI files synthesized using the [Native Instruments' Komplete Complete 12 Ultimite](https://www.native-instruments.com/en/products/komplete/bundles/komplete-12-ultimate/) sample pack with their Kontakt sampling engine. The tracks in **Slakh2100** are split into training (1500 tracks), validation (375 tracks), and test (225 tracks) subsets, and totals 145 hours of mixtures.


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

(((BibTex entry here)))



## Comparison to Other Datasets <a name="comparison"></a>

Here is a comparison between Slakh2100 and the most popular datasets used for audio source separation. The dataset size is recorded in terms of hours of mixture data. Note that MUSDB18 combines data from DSD100 and MedleyDB, which is denoted with an asterisk (\*) below.


| Dataset       | # Songs  | Size (h) | # Tracks | # Instrument Categories |
|---------------|----------:|----------:|----------:|-------------------------:|
| iKala [2]     | 306      | 2        | 2        | 2                       |
| MIR-1k [3]    | 110      | 2.25     | 2        | 2                       |
| DSD100* [4]   | 100      | 7        | 4        | 4                       |
| MedleyDB* [5] | 122      | 7.2      | 1-26     | 82                      |
| MUSDB18* [6]  | 150      | 10       | 4        | 4                       |
| **Slakh2100** | **2100** | **145**  | **4-48** | **34**                  |



Here is how Slakh2100 compares to the full Lakh MIDI Dataset (`lmd-full`) and to the subset of Lakh that has the four instrument categories as described in [Selection from the Lakh MIDI Dataset](#selection) (this is denoted as "LMD (subset)" in the table below).


| Dataset               | # Songs  | Size (h) | # Tracks | # Instrument Categories |
|-----------------------|----------:|----------:|----------:|-------------------------:|
| LMD (subset)          | 20,371   | 1,793    | 4+       | 129                     |
| Lakh MIDI Dataset [1] | 176,581  | 10,521   | 1+       | 129                     |
| **Slakh2100**         | **2100** | **145**  | **4-48** | **34**                  |



## Construction of Slakh <a name="construction"></a>

### Selection from the Lakh MIDI Dataset <a name="selection"></a>

Slakh uses the MIDI instrument program numbers to determine how each MIDI instrument is mapped to each Kontakt patch for synthesis. Slakh2100 contains only MIDI files that have _at least_ piano, bass, guitar, and drums, where each of these four instruments plays at least 50 notes. This subset contains 20,371 songs and is denoted as "LMD (subset)" in the table in the [comparison above](#comparison). From this subset, 2100 songs were randomly selected to be synthesized.

### Rendering and Mixing <a name="rendering"></a>

After the 2100 MIDI files are selected, each MIDI file is split up into its individual "tracks" that contain one instrument per track. These tracks are randomly assigned to an associated Kontakt patch and rendered. Slakh uses 187 Kontakt patches categorized into 34 classes. The patch names and categorizations are available in the [Downloads](#downloads) section, above. Many patches are rendered with effects applied, like reverb, EQ, and compression. MIDI program numbers that are sparsely represented in LMD, like those under the “Sound Effects” header, are omitted. All tracks are rendered into separate monaural audio files at CD quality: 44.1kHz, 16-bit. They are distributed in the [flac](https://xiph.org/flac/) format.

After being rendered, each song is automatically mixed by normalizing each track to be equal in terms of integrated loudness as calculated by the algorithm defined in the ITU-R BS.1770-4 specification [7]. Each track is then summed together instantaneously to make a mixture, and a uniform gain is applied to the mixture and each track to ensure that there is no clipping.

### Flakh2100 <a name="flakh"></a>

The same 2100 MIDI files selected for Slakh2100 are also rendered with [FluidSynth](http://www.fluidsynth.org/) using the `TimGM6mb.sf2` sound font, the default in [pretty\_midi](https://craffel.github.io/pretty-midi/). We refer to the resulting dataset as Flakh. As before, we similarly split the MIDI into individual tracks and render these individually to make stems. But differences in dithering between Kontakt and FluidSynth made normalizing and mixing Flakh as we did above. Instead FluidSynth renders the whole, unsplit MIDI “mixture” as the resultant audio mixture.

## Anaylsis <a name="analysis"></a>

## Examples <a name="examples"></a>

## Benchmarks <a name="benchmarks"></a>


(((Results from benchmark tests coming soon)))


## Citations <a name="citations"></a>

[1] Colin Raffel. "Learning-Based Methods for Comparing Sequences, with Applications to Audio-to-MIDI Alignment and Matching". PhD Thesis, 2016.

[2] T.-S. Chan, T.-C. Yeh, Z.-C. Fan, H.-W. Chen, L. Su, Y.-H. Yang, and R. Jang, “Vocal activity informed singing voice separation with the iKala dataset,” in _Proc. IEEE International Conference on Acoustics, Speech and Signal Processing (ICASSP)_, April 2015, pp. 718–722.

[3] C.-L. Hsu and J.-S. Jang, “On the improvement of singing voice separation for monaural recordings using the MIR-1K dataset,” _IEEE Transactions on Audio, Speech, and Language Processing_, vol. 18, no. 2, pp. 310–319, Feb. 2010.

[4] A. Liutkus, F.-R. Stöter, Z. Rafii, D. Kitamura, B. Rivet, N. Ito, N. Ono, and J. Fontecave, “The 2016 signal separation evaluation campaign,” in _Proc. International Conference on Latent Variable Analysis and Signal Separation (LVA)_. Springer, 2017, pp. 323–332.

[5] R. M. Bittner, J. Salamon, M. Tierney, M. Mauch, C. Cannam, and J. P. Bello, “MedleyDB: A multitrack dataset for annotation-intensive MIR research,” in _Proc. International Society for Music Information Retrieval Conference (ISMIR)_, vol. 14, 2014, pp. 155–160.

[6] Z. Rafii, A. Liutkus, F.-R. Stöter, S. I. Mimilakis, and R. Bittner, “The MUSDB18 corpus for music separation,” Dec. 2017. [Online]. Available: https://doi.org/10.5281/ zenodo.1117372

[7] Recommendation ITU-R BS.1770-4, “Algorithms to measure audio programme loudness and true-peak audio level,” 2017.
