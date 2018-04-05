# TED gender-annotated dataset:

This repository contains the gender-annotated dataset based on [TED](https://ted.com) which was used for gender classification of translated data in Mirkin et al., 2015 and Rabinovich et al., 2017.
As the above papers link to a web page that is no longer available, the dataset is posted here, with consent. If you use this data please cite this paper:

Mirkin, Shachar, Nowson, Scott, Brun, Caroline, & Perez, Julien. (2015) Motivating Personality-aware Machine Translation. In the Proceedings of the Conference on Empirical Methods in Natural Language (EMNLP) 2015, Lisbon, Portugal. [PDF](http://aclweb.org/anthology/D/D15/D15-1130.pdf) [BIB](http://www.emnlp2015.org/proceedings/EMNLP/bib/EMNLP130.bib)

## Data 

The data contains 2 files:
- xrce-emnlp-gender-en.csv - the English data
- xrce-emnlp-gender-fr.csv - the French data

### Data format

The provided data contains the URL to each talk alongsidhe e tannotated gender. The labels are one of "male" "female" and "discard" (to be explained below). For example

```
female,http://www.ted.com/talks/majora_carter_3_stories_of_local_ecoactivism.html
male,http://www.ted.com/talks/chris_gerdes_the_future_race_car_150mph_and_no_driver.html
```
 
## Annotation process

Below is a detailed description of the process used for creating the dataset used in the above paper. This description contains more details than we were able provide in the paper due to space limitations.

### English TED talks

We used the English-French (en-fr) data of the [IWSLT 2014 Evaluation Campaign](https://wit3.fbk.eu/mt.php?release=2014-01)'s MT track, which includes parallel corpora from TED talks transcripts, [WIT3](https://wit3.fbk.eu). From the provided en-fr parallel corpus we extracted all sentences not enclosed within XML tags, i.e. the actual contents of the talks, excluding their titles and descriptions. The en-fr parallel corpus consists of 1415 talks, with approximately 190 thousand sentence-pairs and 3 million tokens for each of the source and target sides (before preprocessing).

We (the first three authors of the paper) annotated the gender of each speaker with a simple web interface ,showing the URL and requiring choosing one of the 3 possible labels. Most URLs of the English TED talks follow a naming pattern that includes the name of the speaker, along with a short title of the talk. This often allows the determination of the gender of the speaker without accessing the link (e.g. Arthur, Sir Ken Robinson vs. Deborah, Elizabeth). Whenever we were not sure about the speaker's gender, we followed the link to verify it.

Any talk with multiple speakers or where the majority of the talk is not a speech, but rather a performance, was discarded ([example](https://web.archive.org/web/20170428020659/http://www.ted.com/talks/raul_midon_plays_everybody_and_peace_on_earth.html)). We explicitly checked any talk with less than 10 lines of text. This allows identifying songs, for instance, which were transcribed over a single line. Note that we discard songs, even if they include texts, since they may have not been written by the performers themselves and since they represent a different genre altogether. We also discard talks with multiple speakers of the same gender to make the dataset appropriate for future analysis.

Out of the 1415 talks, 1012 (72%) were annotated as male, 344 (24%) as female and 59 were discarded.


### French TEDx talks

For our experiments we were looking for data where the source is native French, and which included manual translation into English. The WIT3 data seemingly includes data in the French to English (fr-en) direction as well. However, in practice, TED hosts only talks in English and all "foreign" to English corpora were collected from the translated versions of the site (a search by language retrieves talks that were translated into that language).

We therefore turned to TEDx to obtain French to English data. TEDx are independent TED-style events organized in different locations around the world, often including talks in languages other than English. Unlike English to French, there is no easily accessible parallel data available for French to English, where the source is native French. Hence, we applied the following procedure to collect the necessary data:

We used [Google YouTube Analytics API](https://developers.google.com/youtube/analytics) to search for videos of talks in French, and downloaded their manual French and English subtitles. We have extracted a list of [TEDx](https://www.ted.com/watch/tedx-talks) events in France and their dates via http://www.tedxenfrance.fr (accessed on 23/2/15). Each event-name and year was used as a query, e.g. "TEDx Paris 2011", "TEDx Paris 2011" or "TEDxSciencesPoReims 2015". That, in addition to the query "Tedx francais". Through this process we collected titles and IDs of potential TEDx talks in French.

We downloaded the manual French subtitles of each of the talks and the English manual translation, if they existed, using the [getyoutubecc script](https://github.com/ihinojal/getyoutubecc). 
We used only videos that have both English and French **manual** subtitles (for some YouTube videos, automatic captions are also available), and converted the subtitles srt files, that include the segment number and time, into regular text files. 
We then post-processed the texts to remove any text in parentheses, as this typically included references to the audience like 'Applaudissements' or 'Applause' or transcriber clarifications such as 'CRI (Centre de recherches interdisciplinaires)'. The captions often split sentences in the middle. We therefore merged the captions into a single stream of text, and re-split them into sentences using the XIP parser (Ait-Mokhtar et al., 2001). This process yielded 88 potential talks with parallel texts. Note that this is a parallel corpus, but it is not sentence-aligned. However, since we did not use it to train an SMT model such alignment was not necessary.

These files were annotated using the same process described above for en-fr, with the exception that now URLs did not include the name of the speaker and had to be always followed . We discarded talks by the same criteria as above, with the addition of discarding any talk not delivered in French.

We have explored the option to use language identification for the purpose of determining the language of the video according to the language of its title. This proved unreliable since titles are sometimes short, contain names (e.g. a local name in an English title) or are comprised of a mix of words in the two languages; further, there is not always a match between the language of the title and that of the video.

This resulted with a corpus of 61 talks of which 32 are annotated as male and 29 as female, and 27 were discarded. This is a much higher discard rate in comparison to the English videos, due to the different nature of the TED vs. TEDx events, and the data collection methods.


## References:
- Shachar Mirkin, Scott Nowson, Caroline Brun and Julien Perez. Motivating Personality-aware Machine Translation. In Proceedings of the Conference on Empirical Methods in Natural Language (EMNLP) 2015, Lisbon, Portugal. 
- Ella Rabinovich, Raj Nath Patel, Shachar Mirkin, Lucia Specia and Shuly Wintner. Personalized Machine Translation: Preserving Original Author Traits. In Proceedings of the European Chapter of the Association for Computational Linguistics (EACL) 2017, Valencia, Spain.
- Salah Ait-Mokhtar, Jean-Pierre Chanod, Claude Roux. A multi-input dependency parser. In Proceedings of the Seventh IWPT (International Workshop on Parsing Technologies) 2001, Beijing, China.