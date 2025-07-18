
- Break sections out of stats of online experiment one by one. Finish and move on.
	- [x] Setup
	- [x] Experiments
	- [x] Unbiasedness
	- [x] Variance and standard error
	- [ ] Hypothesis testing
	- [ ] Power

Bonus
- Consider adding introduction.md stuff to index.md
- testing_additional_material

- [ ] Add section on power calculations
    - [ ] Incorporate all metrics types to power calculation (see hesterberg2024power)
    - [ ] Differentiate between relative and abs MDES (default is abs)
    - [ ] Derive power formula
        - [x] From first princples
        - [ ] Starting from alpha and beta
        - [ ] Visual Bloom approach
    - Derive shortcut formulas
        - [ ] Section on 16 vs 32 confusion
    - [ ] Clean up rest of chapter
    - [ ] Power for different metric types (define continuous, proportion, ratio, etc. metrics -- how do they affect calculations?)
    - [ ] Build power calculator (check Statsig one — but add unbalanced samples option), build beautiful visualisations (use uv for package mgt)

- [ ] Add section on testing and CI
- [ ] Show that CLT is justified so we can do inference
	- [ ] Imbens Rubin
	- [ ] wager
	- [ ] ding2023first (Chapter 4, Theorem 4.2), li2017general

- [ ] Show that sigma_hat is unbiased estimator of sample variance sigma 
	- [ ] Imbens and Rubin (Appendix 6.A)
	- [ ] ding2023first lemma c4

- [ ] Write chapter and consider turning it into paper (check with Max for relevant outlet if needed)

- [ ] Problems/cases that help more deeply understand the approach
	- [ ] Lords/Simpson paradox (see simpsons_paradox)
	- [ ] Presence of concomitant variable (rubin2005causal section 7)


Later
- [ ] Read titiunik2020natural for neat definition of RCE and interesting discussion of types of randomness
- [ ] deng2017trustworthy on more general/complex cases in online experiment practice 

- Integrate Kohavi advanced topics into book: https://docs.google.com/document/d/12iy35z_aM5r76gqgq5f-mzVRwZoHjz4kBjv3rMULSEw/edit?tab=t.0#heading=h.3qu16rq6g769 

- Integrate this into book: https://www.linkedin.com/posts/matheus-facure-7b0099117_alternative-experiment-designs-activity-7247577102797938688-s31_/?utm_source=share&utm_medium=member_desktop


Even later:
- [ ] Add super population case for ramp-up experiments (start with wager2024causal)
- [ ] Read and integrate Holland and Rubin 1983 on Lord paradox
- [ ] Read and integrate seminal papers by Rubin as mentioned in holland1986statistics
- [ ] Integrate: https://dspace.mit.edu/bitstream/handle/1721.1/84065/Yamamoto_Unpacking%20the%20Black.pdf?sequence=1&isAllowed=y

- [ ] Read and integrate aronow2013class as relevant
- [ ] Read and integrate abadie2020sampling as relevant
- [ ] Read and integrate freedman2008statistical

Experiment design
- [ ] Incorporate box1978statistics
- [ ] Incorporate cox2000theory
- Susan Athey, designing complex experiments
  - [Lecture on complex experiments (incl. MABs)](https://www.youtube.com/watch?v=I6GyDWh8kfw)
  - athey2024designing, slides to the above lecture

- Integrate Kohavi advanced topics into book: https://docs.google.com/document/d/12iy35z_aM5r76gqgq5f-mzVRwZoHjz4kBjv3rMULSEw/edit?tab=t.0#heading=h.3qu16rq6g769 
- Integrate alternative experiment designs from Matheus Facure: https://docs.google.com/presentation/d/1cyRKkxvOAosMzlIVnSIGfZf63nq7PwZ9zaL5NZEn19c/edit?pli=1#slide=id.g1e5c694a008_0_52, and discussion here: https://www.linkedin.com/posts/matheus-facure-7b0099117_alternative-experiment-designs-activity-7247577102797938688-s31_?utm_source=share&utm_medium=member_desktop

- Integrate pervasive experimentation problems: https://statmodeling.stat.columbia.edu/2024/06/20/pervasive-randomization-problems-here-with-headline-experiments/

- [ ] Add discussion on model vs randomisation based inference
- [ ] Bayesian experimentation
- [ ] Regression
- [ ] Regression approach to experiments
    - [ ] Friedman critique
    - [ ] Lin response
    - [ ] Imbens and Rubin approach
- [ ] CUPED
    - [ ] Connection to DiD


- [ ] Power of random experiments (post 2)

- [ ] Variance reduction notes from JET project

- [ ] CUPED vs regression adjustment discussion 
    - [ ] Friedman critique of OLS and Lin response (and Imbens and Rubin take)
    - [ ] My notes on regression adjustment ignore demeaning (y tilde is alpha hat plut betahat times x, so not exactly the same as cuped, but has no effect because alpha is demeaning only and has no effect on cov and var. Write down)
    - [ ] Individual-level adjustment vs group adjustment (-x_i vs -\bar{x}) See Bookings post on this.
    - [ ] CUPED in practice: all the important decisions you have to make nobody talks about.


- [ ] FDR vs family-wise-false-positive rate

- [ ] Always-valid p-values

- [ ] Add visualisations
    - [ ] Find suitable library (tikz? 3b1b?)

- [ ] Why is qe power lower than experiment power? must be due to higher sampling variance. how come?

- [ ] Incorporate relevant stuff from Spotify posts: https://engineering.atspotify.com/tag/experimentation/

- [ ] Read Imbens and Athey papers

- imbens2020potential - up to section 2.4

- athey2019machine

- Marton Trencseni posts on a/b testing (https://bytepawn.com/tag/ab-testing.html)

- Lukas Vermeer posts (https://www.lukasvermeer.nl/publications/)

- [ ] Gelman et al chapters

- [ ] Quasi-experimental methods - Angrist and Pischke book

- [ ] Cox and Reid book (https://www.amazon.co.uk/Experiments-Chapman-Monographs-Statistics-Probability-ebook/dp/B00UVA67EC/ref=tmm_kin_swatch_0?_encoding=UTF8&qid=1673947989&sr=8-1)

- [ ] Business data science (https://www.lukasvermeer.nl/publications/)
Stuff to consider

- ML and causal inference Mixtape course

- Machine learning and econometrics (Athey and Imbens lectures - https://www.aeaweb.org/conference/cont-ed/2018-webcasts)

- [ ] bojinov2023design section 5 for cool approach to experiment simulation that I might want to add to rosalie.

## Done

- [x] Clean up stats of online experiments note
- [x] Find content from old stats_foundation chapter (or copy from github archive), create again, add indpendence vs uncorrelated, continue with derivation (talk about why we need assumption that Y_i fixed – if correlated with D_i, then e of estimator not zero)
- [x] Add bday derivation notes to Neyman chapter (use Imbens and Rubin book notes as starting point and move neyman and bday derivation there so I still have neyman chapter`)
- [x] Neyman unbiasedness and variance derivation for CRE
	- [x] Imbens and Rubin proof (Appendix 6.A)
	- [x] ding2023first proof (Chapter 4)
- [x] Write appendix files to summarise relevant content from each of the following (get info from as many sources as I need in order to have all I need for stats of experiments):
  - [x] athey2017econometrics
  - [x] imbens2015causal
- [x] Get internal preview to work
- [x] Complete reboot of mac (if works well after, keep, else buy new macbook)
- [x] CI notes on effect of below on experiments (for meta interview prep)
    - [x] Seasonality
    - [x] Network effects
    - [x] Cannibalisation
- [x] Set up provisional book structure
- [x] Move existing material from other places
- [x] Push to Github
- [x] Publish book on website -- gonna wait with that until I have some content
- [x] Projection
    - [x] Create outline on paper
    - [x] Solve Quarto preview in vscode (workaround using localhost in simple browser inside vscode, but need to refresh manually)
    - [x] Solve math limitations -- rmd works (10 theorems, so quarto should, too) -- use quarto book
    - [x] Run cells in quarto book
    - [x] Projection in general
