# versions
The Versions dataset was created from Wikipedia revision history.  The dataset can be used for machine learning to determine version relation amongst documents.

We took Wikipedia revision dumps that contain all revisions of Wikipedia pages on a certain topic. Each revision is accompanied by some metadata (ex. timestamp, parent_id, etc.). To ensure that we considered versions that were sufficiently different from each other, we used timestamps in an automated filtering process to create the dataset.

Each edit made by a user to a Wikipedia page can be considered a separate version of that Wikipedia page (these are referred to as revisions in the Wikipedia terminology). However, there are often some artifacts in the revisions – for example, troll edits are insincere edits made by users. We observed that these edits do not last, and the page is reset to the last relevant version by the moderators within a short time period. Based on this observation, we removed all edits that do not last more than 𝐴𝑉𝐸/10 seconds, where 𝐴𝑉𝐸 is defined as the average time between successive edits for a given Wikipedia page. Instead of setting a global threshold, this threshold varies for every page because some pages are edited more frequently than others.

We used timestamps along with metadata for the purpose of filtering. Next, we filtered out pages that were rarely edited. This was done by setting a hard threshold – i.e., pages should consist of at least 10 versions. After this we stochastically sampled ⌊𝑈 (𝑚𝑖𝑛,𝑤𝑖𝑑𝑡ℎ)⌋ revisions from all versions, U is the uniform distribution, 𝑚𝑖𝑛 is the minimum number of samples, 𝑚𝑖𝑛 +𝑤𝑖𝑑𝑡ℎ + 1 is the maximum number of samples, and ⌊.⌋ is the greatest integer function. We took 𝑚𝑖𝑛 to be 7 and 𝑤𝑖𝑑𝑡ℎ as 5. However, since it is still possible that the set of revisions obtained via uniform sampling don’t have considerable differences between them, we stochastically filtered out more revisions to encourage differences between them. This was done by having higher probability of sampling versions which have more difference in terms of length of the documents with the previously selected versions. This increases the probability of having a set of versions that differ more in content than other sets.

Once we obtained the list of pages and the versions being considered for each of them, we extracted the Wikipedia article content corresponding to each version. We removed all additional information in sections like References, Category, etc. from the articles. This was done to ensure that the model didn’t utilize this information to learn undesirable patterns, and to ensure that the identification of relations was solely based on the content.

Our final dataset consists of 446 unique Wikipedia articles with a total of 2508 versions; each article having 5.62 versions on average. We split the dataset into train, validation, and test sets in the ratio of 0.7 : 0.1 : 0.2, respectively.

The directories inside `version_documents` correspond to a unique article present in this dataset. Each directory contains text files with the naming convention: `0.txt`, `1.txt`, .... While `0.txt` represents the oldest version of the respective article, the text file with largest number in its name represents the newest one. For convenience, we have also provided the parsed dataset at `parsed_dataset.pkl`. On loading, this gives a list of 2-tuple `(title, revisions)`, where `title` is the name of the article and `revisions` is the list of versions arranged in chronological order.

The original dataset used in the paper was constructed using these Wikipedia dump files:
- [enwiki-20200401-pages-meta-history1.xml-p1p908](https://dumps.wikimedia.org/enwiki/20200401/enwiki-20200401-pages-meta-history1.xml-p1p908.7z)
- [enwiki-20200401-pages-meta-history1.xml-p13958p14691](https://dumps.wikimedia.org/enwiki/20200401/enwiki-20200401-pages-meta-history1.xml-p13958p14691.7z)

However, these files are currently unavailable in [wikimedia](https://dumps.wikimedia.org/enwiki/). We request contributors to contact us if they have access to these files. Acquiring these files will allow us to construct a complete dataset with consistent statistics as reported in the paper.

## Contact 
[Natwar Modani](mailto:nmodani@adobe.com), [Inderjeet Nair](mailto:inair@adobe.com)

## Citation
This work is accepted at International conference on [Web Information Systems Engineering (WISE), 2021](http://www.wise-conferences.org/2021/index.html).

  
## License Information
<a rel="license" href="http://creativecommons.org/licenses/by-sa/3.0/"><img alt="Creative Commons License" style="border-width:0" src="https://licensebuttons.net/l/by-sa/3.0/88x31.png" /></a><br />This dataset is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by-sa/3.0/">Creative Commons Attribution-ShareAlike 3.0 Unported license</a>.

NOTE: _Please refer to the [LICENSE](./LICENSE.md) file for detailed information._
