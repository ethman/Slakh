<style>

td, th {
  border: 1px solid #aaa;
  padding: 6px;
}

tr:hover {background-color: #ddd;}

th {
  padding-top: 12px;
  padding-bottom: 12px;
  text-align: center;
}

table {
    border: 1px solid #aaa;
}
</style>


The Synthesized Lakh (Slakh) Dataset is a dataset of multi-track audio and aligned MIDI for music source separation and multi-instrument automatic transcription. Individual MIDI tracks are synthesized from the Lakh MIDI Dataset v0.1 using professional-grade sample-based virtual instruments, and the resulting audio is mixed together to make musical mixtures. This release of Slakh, called Slakh2100, contains 2100 automatically mixed tracks and accompanying, aligned MIDI files, synthesized from 187 patches categorized into 34 classes.

Slakh is brought to you by [Mitsubishi Electric Research Lab (MERL)](http://www.merl.com/) and the [Interactive Audio Lab at Northwestern University](http://music.cs.northwestern.edu/).


## Table of Contents

1. [Get Slakh2100](#download)
    <br>&emsp;a. [Slakh2100 at a Glance](#glance)
2. [Comparison to Other Datasets](#comparison)
3. [License and Attribution](#license)
4. [Construction of Slakh](#construction)
    <br>&emsp;a. [Selection from the Lakh MIDI Dataset](#selection)
    <br>&emsp;b. [Rendering and Mixing](#rendering)
    <br>&emsp;c. [Flakh2100](#flakh)<br>
5. [Analysis](#analysis)
6. [Audio Examples](#examples)
7. [Benchmarks](#benchmarks)
8. [Issues or Questions](#issues)
9. [Citations](#citations)
 
 

## Get Slakh2100 <a name="download"></a>

**Slakh2100 dataset:** [Slakh2100 is available at Zenodo.org. Click here to download.](https://zenodo.org/record/4599666)

[Also, here is a link to a tiny subset of Slakh2100 (useful for prototyping).](https://zenodo.org/record/4603870)

Support code for Slakh: [Available here.](https://github.com/ethman/slakh-utils) 

Code to render Slakh data: [Available in this repo.](https://github.com/ethman/slakh_generation_scripts) 


### Slakh2100 at a glance <a name="glance"></a>

See [the dataset at a glance](https://github.com/ethman/slakh-utils#at-a-glance), and [info about `metadata.yaml`](https://github.com/ethman/slakh-utils#metadata).

**Important: Some MIDI files are replicated (due to a bug), see [this section](https://github.com/ethman/slakh-utils/blob/master/README.md#make-splits) for the fix depending on your use case.**


## License and Attribution <a name="license"></a>
  
If you use Slakh2100/Flakh2100 or generate data using the same method we ask that you cite it using the following bibtex entry:

```
@inproceedings{manilow2019cutting,
  title={Cutting Music Source Separation Some {Slakh}: A Dataset to Study the Impact of Training Data Quality and Quantity},
  author={Manilow, Ethan and Wichern, Gordon and Seetharaman, Prem and Le Roux, Jonathan},
  booktitle={Proc. IEEE Workshop on Applications of Signal Processing to Audio and Acoustics (WASPAA)},
  year={2019},
  organization={IEEE}
}

```

Slakh2100 and Flakh2100 are licensed under a <a rel="license" href="http://creativecommons.org/licenses/by/4.0/">Creative Commons Attribution 4.0 International License</a>.



## Comparison to Other Datasets <a name="comparison"></a>

Here is a comparison between Slakh2100 and the most popular datasets used for audio source separation. The dataset size is recorded in terms of hours of mixture data. Note that MUSDB18 combines data from DSD100 and MedleyDB, which is denoted with an asterisk (\*) below.


| Dataset       | # Songs  | Size (h) | # Tracks | # Instrument Categories |
|:--------------|---------:|---------:|---------:|------------------------:|
| iKala [2]     | 306      | 2        | 2        | 2                       |
| MIR-1k [3]    | 110      | 2.25     | 2        | 2                       |
| DSD100* [4]   | 100      | 7        | 4        | 4                       |
| MedleyDB* [5] | 122      | 7.2      | 1-26     | 82                      |
| MUSDB18* [6]  | 150      | 10       | 4        | 4                       |
| **Slakh2100** | **2100** | **145**  | **4-48** | **34**                  |

<br>

![comparison with other datasets](https://github.com/ethman/Slakh/raw/master/img/slakh_comp1.png)


Here is how Slakh2100 compares to the full Lakh MIDI Dataset and to the subset of Lakh that has sources with at least one source from each Piano, Bass, Guitar, and Drums, as described in [Selection from the Lakh MIDI Dataset](#selection) (this is denoted as "LMD (subset)" in the table below).


| Dataset               | # Songs  | Size (h) | # Tracks | # Instrument Categories |
|:----------------------|---------:|---------:|---------:|------------------------:|
| LMD (subset)          | 20,371   | 1,793    | 4+       | 129                     |
| Lakh MIDI Dataset [1] | 176,581  | 10,521   | 1+       | 129                     |
| **Slakh2100**         | **2100** | **145**  | **4-48** | **34**                  |

<br>

![comparison with other datasets, and expansion](https://github.com/ethman/Slakh/raw/master/img/slakh_comp2.png)

There is lots of room to grow to add more mixtures from MIDI files, as well as adding new sampling libraries to get a richer dataset of sounds. For more analysis, see our paper presented at WASPAA 2019.


## Construction of Slakh <a name="construction"></a>

### Selection from the Lakh MIDI Dataset <a name="selection"></a>

Slakh uses the MIDI instrument program numbers to determine how each MIDI instrument is mapped to each patch for synthesis. Slakh2100 contains only MIDI files that have _at least_ piano, bass, guitar, and drums, where each of these four instruments plays at least 50 notes. This subset contains 20,371 songs and is denoted as "LMD (subset)" in the table in the [comparison above](#comparison). From this subset, 2100 songs were randomly selected to be synthesized.

### Rendering and Mixing <a name="rendering"></a>

After the 2100 MIDI files are selected, each MIDI file is split up into its individual "tracks" that contain one instrument per track. These tracks are randomly assigned to an associated patch and rendered. Slakh uses 187 patches categorized into 34 classes. The patch names and categorizations are available in the [Downloads](#downloads) section, above. Many patches are rendered with effects applied, like reverb, EQ, and compression. MIDI program numbers that are sparsely represented in LMD, like those under the “Sound Effects” header, are omitted. All tracks are rendered into separate monaural audio files at CD quality: 44.1kHz, 16-bit. They are distributed in the [flac](https://xiph.org/flac/) format.

After being rendered, each song is automatically mixed by normalizing each track to be equal in terms of integrated loudness as calculated by the algorithm defined in the ITU-R BS.1770-4 specification [7]. Each track is then summed together instantaneously to make a mixture, and a uniform gain is applied to the mixture and each track to ensure that there is no clipping.

### Flakh2100 <a name="flakh"></a>

The same 2100 MIDI files selected for Slakh2100 are also rendered with [FluidSynth](http://www.fluidsynth.org/) using the `TimGM6mb.sf2` sound font, the default in [pretty\_midi](https://craffel.github.io/pretty-midi/). The resulting dataset is referred to as **Flakh2100**. As before, the MIDI file is split into individual tracks that were rendered individually to make stems. But low-amplitude noise added by FluidSynth (most likely for dithering) can be boosted to unnaturally loud levels when normalizing and mixing Flakh as we did for Slakh. This made normalizing and mixing Flakh in the same way as Slakh impossible. Instead FluidSynth renders the whole, unsplit MIDI “mixture” as the resultant audio mixture. Similarly, these tracks are rendered into separate monaural audio files at CD quality: 44.1kHz, 16-bit and they are distributed in the [flac](https://xiph.org/flac/) format.

## Anaylsis <a name="analysis"></a>

#### Analysis of MIDI Files Selected for Slakh2100

Here is the number of _mixtures_ that contain an instrument in a given MIDI instrument program "family":

![number of mixtures with class](https://github.com/ethman/Slakh/raw/master/img/n_mix_with_inst_class.png)

And here is the number of _tracks_ by MIDI instrument program "family":

![number of tracks with class](https://github.com/ethman/Slakh/raw/master/img/inst_cls.png)

[Here is a link to a larger image that contains the number of tracks rendered with a given MIDI instrument program number.](https://github.com/ethman/Slakh/raw/master/img/inst_pgm.png)


#### Analysis of Audio

Here are the averages of the spectra of each of the MIDI instrument program "families" as they are rendered in Slakh2100:

![slakh instrument spectra](https://github.com/ethman/Slakh/raw/master/img/slakh_src_by_class_big.png)

Here is a comparison of the average mixture spectra of Slakh2100, Flakh2100 (denoted as Slakh2100-FS in the graph), and MUSDB18 [6]:

![spectra comparison overlayed](https://github.com/ethman/Slakh/raw/master/img/all_spectra_placeholder.png)

Here are those same three average mixture spectra intentionally separated along the y-axis for clarity:

![spectra comparison](https://github.com/ethman/Slakh/raw/master/img/all_spectra_option3.png)

Here is a [UMAP](https://github.com/lmcinnes/umap) projection of the spectra of each mixture in Slakh2100, Flakh2100, and MUSDB18:

![umap](https://github.com/ethman/Slakh/raw/master/img/all_umap_trunc_option2.png)

<br><br>


## Audio Examples <a name="examples"></a>

Here are two examples from each dataset:

|    | Slakh2100   | Flakh2100  |
|----|-------------|------------|
| Track00798 &emsp;| <audio src="https://github.com/ethman/Slakh/raw/master/audio/slakh_00798.mp3" controls> </audio>  |  <audio src="https://github.com/ethman/Slakh/raw/master/audio/flakh_00798.mp3" controls> </audio>   |
| Track01209 &emsp;| <audio src="https://github.com/ethman/Slakh/raw/master/audio/slakh_01209.mp3" controls> </audio>  |  <audio src="https://github.com/ethman/Slakh/raw/master/audio/flakh_01209.mp3" controls> </audio>   |

<br>

There is no getting around the fact that these are, in fact, MIDI files. One thing that we have determined is that it's impossible to make the audio not sound cheesy if the MIDI files themselves are cheesy. But the richness and "realness" of the sample-based virtual instruments is what powers the better separation in deep learning contexts.

<br>

## Benchmarks <a name="benchmarks"></a>

We ran a set of benchmark tests on Slakh2100, Flakh2100, and MUSDB18 using different types of mixing. For full details see our WASPAA paper.

Bass and drums separation performance in terms of SI-SDR (dB)[8] averaged over the MUSDB18 test set for the unprocessed mixture, models trained on various datasets, and oracle methods.

|                           | Training data [h] | Bass | Drums |
|---------------------------|:-----------------:|:----:|:-----:|
| Unprocessed mixture       |         -         | -6.0 |  -3.8 |
| MUSDB18                   |         5         | -0.5 |  2.2  |
| MUSDB18 + Slakh           |         48        |  1.3 |  3.6  |
| MUSDB18 + Flakh           |         48        |  0.0 |  3.1  |
| MUSDB18 + MUSDB18-incoh.  |         48        |  1.2 |  3.5  |
| Slakh redux               |         5         | -2.0 |  -0.6 |
| Slakh                     |         43        | -2.4 |  -0.7 |
| **Oracle Methods:**       |                   |      |       |
| IBM                       |         -         |  5.9 |  8.4  |
| IRM                       |         -         |  5.7 |  8.1  |
| Wiener-like               |         -         |  6.9 |  9.1  |
| Truncated phase sensitive |         -         |  7.9 |  10.1 |

<br>
Separation performance in terms of SI-SDR (dB)[8] averaged over the Slakh2100 test set for the unprocessed mixture, models trained on various datasets, and oracle methods.

|                           | Training data [h] |  Bass | Drums | Guitar | Piano |
|---------------------------|:-----------------:|:-----:|:-----:|:------:|:-----:|
| Unprocessed mixture       |         -         |  -6.5 |  -6.6 |  -6.9  |  -8.0 |
| MUSDB18                   |         5         |  -3.2 |  3.2  |    -   |   -   |
| MUSDB18 + Slakh           |         48        |  3.3  |  9.0  |    -   |   -   |
| MUSDB18 + Flakh           |         48        | -10.2 |  4.1  |    -   |   -   |
| MUSDB18 + MUSDB18-incoh.  |         48        |  0.7  |  5.1  |    -   |   -   |
| Slakh redux               |         5         |  3.3  |  9.4  |  -3.0  |  -3.1 |
| Slakh                     |         43        |  3.9  |  9.9  |  -1.8  |  -2.2 |
| **Oracle Methods:**       |                   |       |       |        |       |
| IBM                       |         -         |  5.3  |  10.0 |   5.2  |  4.1  |
| IRM                       |         -         |  5.2  |  9.7  |   5.4  |  4.3  |
| Wiener-like               |         -         |  6.3  |  10.7 |   6.4  |  5.3  |
| Truncated phase sensitive |         -         |  7.3  |  11.7 |   7.3  |  6.4  |

<br><br>

## Issues or Questions <a name="issues"></a>

If there are any issues or questions with downloading Slakh2100 or with the dataset itself, please open an issue [here](https://github.com/ethman/Slakh/issues).

If there are any issues with the support code, please open an issue in [this repo](https://github.com/ethman/slakh-utils/issues).

## Citations <a name="citations"></a>

[1] Colin Raffel. "Learning-Based Methods for Comparing Sequences, with Applications to Audio-to-MIDI Alignment and Matching". PhD Thesis, 2016.

[2] T.-S. Chan, T.-C. Yeh, Z.-C. Fan, H.-W. Chen, L. Su, Y.-H. Yang, and R. Jang, “Vocal activity informed singing voice separation with the iKala dataset,” in _Proc. IEEE International Conference on Acoustics, Speech and Signal Processing (ICASSP)_, April 2015, pp. 718–722.

[3] C.-L. Hsu and J.-S. Jang, “On the improvement of singing voice separation for monaural recordings using the MIR-1K dataset,” _IEEE Transactions on Audio, Speech, and Language Processing_, vol. 18, no. 2, pp. 310–319, Feb. 2010.

[4] A. Liutkus, F.-R. Stöter, Z. Rafii, D. Kitamura, B. Rivet, N. Ito, N. Ono, and J. Fontecave, “The 2016 signal separation evaluation campaign,” in _Proc. International Conference on Latent Variable Analysis and Signal Separation (LVA)_. Springer, 2017, pp. 323–332.

[5] R. M. Bittner, J. Salamon, M. Tierney, M. Mauch, C. Cannam, and J. P. Bello, “MedleyDB: A multitrack dataset for annotation-intensive MIR research,” in _Proc. International Society for Music Information Retrieval Conference (ISMIR)_, vol. 14, 2014, pp. 155–160.

[6] Z. Rafii, A. Liutkus, F.-R. Stöter, S. I. Mimilakis, and R. Bittner, “The MUSDB18 corpus for music separation,” Dec. 2017. [Online]. Available: https://doi.org/10.5281/ zenodo.1117372

[7] Recommendation ITU-R BS.1770-4, “Algorithms to measure audio programme loudness and true-peak audio level,” 2017.

[8] J. Le Roux, S. Wisdom, H. Erdogan, and J. R. Hershey, “SDR–half-baked or well done?” in Proc. IEEE Interna- tional Conference on Acoustics, Speech and Signal Processing (ICASSP), May 2019, pp. 626–630.
