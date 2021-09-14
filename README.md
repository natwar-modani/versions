# versions
This repository is for sharing the dataset of version detection problem. The dataset is created from Wikipedia revision history.

We took Wikipedia revision dumps that contain all revisions of Wikipedia pages on a certain topic. Each revision is accompaniedby some metadata (like timestamp, parent_id, etc.). To ensure that we consider versions that are sufficiently different from each other, we use timestamps in an automated filtering process to create the dataset.

Each edit made by the user to a Wikipedia page can be considered a separate version of that Wikipedia page (these are referred to as revisions in the Wikipedia terminology). However, oftem there are some artifacts in the revisions – for example, troll edits are insincere edits made by users. We observe that these edits do not last, and the page is reset to the last relevant version by the moderators within a short duration. Based on this observation, we remove all edits that do not last more than 𝐴𝑉𝐸/10 seconds, where 𝐴𝑉𝐸 is defined as the average time between successive edits for a given Wikipedia page. Instead of setting a global threshold, this threshold varies for every page because some pages are edited more frequently than others.

We use timestamps obtained along with metadata for the purpose of filtering. Next, we filter out pages that are rarely edited. This is doneby setting a hard threshold – i.e., pages should consist of at least 10 versions. After this we stochastically sample ⌊𝑈 (𝑚𝑖𝑛,𝑤𝑖𝑑𝑡ℎ)⌋ revisions from all versions, U is the uniform distribution, 𝑚𝑖𝑛 is the minimum number of samples,𝑚𝑖𝑛 +𝑤𝑖𝑑𝑡ℎ + 1 is the maximum number of samples, and ⌊.⌋ is the greatest integer function. We take 𝑚𝑖𝑛 to be 7 and 𝑤𝑖𝑑𝑡ℎ as 5. However, since it is still possible that the set of revisions obtained via uniform sampling don’t have considerable differences between them, we stochastically filter out more revisions to encourage differences between them. This is done by having higher probability of sampling versions which have more difference in terms of length of the documents with the previously selected versions. This increases the probability of having the set of versions that differ more in content.

Once we obtain the list pages and the versions being considered for each of them, we scrape the Wikipedia article corresponding to each version. We remove all additional information in sections like References, Category, etc. from the scraped articles. This is done to ensure that the model doesn’t utilize this information to learn undesirable patterns, and to ensure that the identification ofrelations is solely based on the content.

Our final dataset comprises 446 unique Wikipedia articles with a total of 2508 versions; each article having 5.62 versionson average. We split the dataset into train, validation, and test sets in the ratio of 0.7 : 0.1 : 0.2, respectively.

The original dataset used in the paper was constructed using these dump files:
- [enwiki-20200401-pages-meta-history1.xml-p1p908](https://dumps.wikimedia.org/enwiki/20200401/enwiki-20200401-pages-meta-history1.xml-p1p908.7z)
- [enwiki-20200401-pages-meta-history1.xml-p13958p14691](https://dumps.wikimedia.org/enwiki/20200401/enwiki-20200401-pages-meta-history1.xml-p13958p14691.7z)

However, these files are currently unavailable in [wikimedia](https://dumps.wikimedia.org/enwiki/). We request contributors to contact us in case they have access to these files. Acquiring these files will allow us to construct complete dataset with consistent statistics as reported in the paper.

## Contact 
[Natwar Modani](mailto:nmodani@adobe.com), [Inderjeet Nair](mailto:inair@adobe.com)

## Citation
This work is accepted at International conference on [Web Information Systems Engineering (WISE), 2021](http://www.wise-conferences.org/2021/index.html).

  
## License Information
<a rel="license" href="http://creativecommons.org/licenses/by-nc/4.0/"><img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by-nc/4.0/88x31.png" /></a><br />This dataset is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by-nc/4.0/">Creative Commons Attribution-NonCommercial 4.0 International License</a>.

NOTE: _Please refer to the [LICENSE](./LICENSE.md) file for detailed information._
