<!-- ---
title: "Are anime titles getting longer?"
description: |
  A quick exploratory analysis on the length of anime titles. 
--- -->

# Are anime titles getting longer?

```
db <- officialtitles %>% 
  mutate(ntitle = nchar(title_primary))
anime_origin <- db %>% 
  count(origin) %>% 
  deframe()
dbl <- db %>% 
  arrange(desc(ntitle)) %>% 
  slice(1:25)
```

[AniDB](https://anidb.net/) is a website that hosts extensive information on
anime from China, Japan and Korea. There are currently information on **`r
comma(nrow(db))` anime** of which `r
percent(anime_origin["JP"]/sum(anime_origin))` originated from Japan.

As an anime lover, I've watched over 700 anime (which is still less than 5% in
the whole database!) but one thing I noticed over recent years is that some
anime titles are bizzarely long... or more like anime titles are becoming
sentences. To explore this, I decided to use the
[`anidb`](https://github.com/emitanaka/anidb) R-package to look at the data.

First note that anime titles come in many forms.  For example, 

* "`r db[22,"title_original"]`" is the **Japanese title**,
* "`r db[22,"title_primary"]`" is the **primary title** (the official title in
  the country of origin but in romanized form), and
* "`r db[22,"title_en"]`" is the **English title**. The English title may be
  unavailable if the anime is not licensed for English audiences.

In the following explorations, I use the primary title. 

Figure \@ref(fig:title-length-distribution) shows that the distribution of the
primary title length. We can see that most anime titles are less than 70
characters but there are some Japanese anime title that are double this length.
The top 25 animes have title length greater than `r dbl$ntitle[nrow(dbl)]`
characters.

```
ggplot(db, aes(ntitle)) +
  geom_histogram(binwidth = 1, fill = "#79003e") +
  labs(x = "The length of the romanised primary anime title by county of origin", 
       y = "Count") + 
  facet_grid(origin ~ ., scales = "free_y")
```
![image](https://user-images.githubusercontent.com/17341000/174016396-de43829f-bbdc-4229-9370-455bc8961c6c.png)

The five longest titles from longest to shortest are: 

* "`r dbl$title_primary[1]`"
* "`r dbl$title_primary[2]`"
* "`r dbl$title_primary[3]`"
* "`r dbl$title_primary[4]`"
* "`r dbl$title_primary[5]`"

```
info <- anime_info(dbl$aid) %>% 
  left_join(dbl, by = "aid")
```

Because of AniDB's limit on API call (multiple requests can get you banned
easily -- turns out that the limit is quite small; about 13-14 calls already got
me banned...), I'm going to just study the top 25 anime in terms of title
length.

Figure \@ref(fig:release-date) suggest that super long titles are more common in
the last decade than in the past. But the analysis is only based on top 25 anime
with the longest titles so it could benefit from more extensive study.

```
ggplot(info, aes(start_date, ntitle)) + 
  geom_point(color = "#79003e") + 
  labs(x = "Start date", y = "Title length")
```
![](https://emitanaka.org/posts/2022-01-16-anime-titles/figures/release-date-1.png)
