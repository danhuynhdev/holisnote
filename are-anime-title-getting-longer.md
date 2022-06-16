<!-- ---
title: "Are anime titles getting longer?"
description: |
  A quick exploratory analysis on the length of anime titles. 
--- -->

# Are anime titles getting longer?

```aml
// Get unique primary title

// Anime by origin

// top 25 anime
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

```aml
// Count number of anime by title length with legend is country
@viz
{
    "viz_type": "column_chart",
    "fields": {
        "x_axis": {
            "path_hash": {
                "field_name": "tilte_length",
                "model_id": "public_anime_titles"
            },
            "full_path": null,
            "transformation": null,
            "type": "number",
            "format": {
                "type": "number",
                "format": {
                    "pattern": "inherited"
                }
            },
        },
        "series": {
            "path_hash": {
                "field_name": "language",
                "model_id": "public_anime_titles"
            },
            "type": "text",
            "transformation": null,
            "format": {
                "type": "string",
                "sub_type": "string"
            }
        },
        "y_axes": [
            {
                "columns": [
                    {
                        "path_hash": {
                            "field_name": "aid",
                            "model_id": "public_anime_titles"
                        },
                        "type": "auto",
                        "aggregation": "count",
                        "format": {
                            "type": "number",
                            "format": {
                                "pattern": "inherited"
                            }
                        },
                        "color": "auto",
                        "series_settings": {
                            "series_hash": {
                                "ja": {
                                    "color": "auto",
                                    "hidden": false
                                },
                                "ko": {
                                    "color": "auto",
                                    "hidden": false
                                },
                                "zh-Hans": {
                                    "color": "auto",
                                    "hidden": false
                                },
                            },
                            "palette_id": -2,
                            "series_type": "auto"
                        }
                    }
                ]
            }
        ]
    },
    "settings": {
        "x_axis": {
            "title": null
        },
        "legend": {
            "enabled": true,
            "alignment": "bottom"
        },
        "y_axes": [
            {
                "title": null,
                "align": "left",
                "scale_type": "linear",
                "min": null,
                "max": null,
                "show_data_label": false,
                "show_series_percentage": false,
                "stack_series": false,
                "stack_type": "normal",
                "show_stack_total": false,
                "show_group_total": false,
                "group_long_tail": false,
                "maximum_groups": 5
            }
        ],
        "misc": {
            "custom_color_list": [
                {}
            ],
            "pagination_size": 25,
            "show_row_number": true,
            "row_limit": -1
        },
        "quick_pivot": false,
        "sort": {},
        "pop_settings": null,
        "others": {
            "include_empty_children_rows": false
        }
    },
    "format": {},
    "filters": [
        {
            "operator": "is",
            "values": [
                "ja",
                "zh-Hans",
                "ko"
            ],
            "modifier": null,
            "options": null,
            "path_hash": {
                "model_id": "public_anime_titles",
                "field_name": "language"
            },
            "aggregation": null,
            "label": "Adhoc filter",
            "dynamic_filter_id": null
        },
        {
            "operator": "is",
            "values": [
                "official"
            ],
            "modifier": null,
            "options": null,
            "path_hash": {
                "model_id": "public_anime_titles",
                "field_name": "title_type"
            },
            "aggregation": null,
            "label": "Adhoc filter",
            "dynamic_filter_id": null
        }
    ],
    "adhoc_fields": []
}
;;
```
![image](https://user-images.githubusercontent.com/17341000/174016396-de43829f-bbdc-4229-9370-455bc8961c6c.png)

The five longest titles from longest to shortest are: 

```aml
// Top 5 Title
@viz
{
    "viz_type": "data_table",
    "fields": {
        "table_fields": [
            {
                "path_hash": {
                    "field_name": "title",
                    "model_id": "public_anime_titles"
                },
                "type": "text",
                "format": {
                    "type": "string",
                    "sub_type": "string"
                }
            },
            {
                "path_hash": {
                    "field_name": "tilte_length",
                    "model_id": "public_anime_titles"
                },
                "type": "number",
                "format": {
                    "type": "number",
                    "format": {
                        "pattern": "inherited"
                    }
                }
            }
        ]
    },
    "settings": {
        "misc": {
            "custom_color_list": [
                {}
            ],
            "pagination_size": 25,
            "show_row_number": true,
            "row_limit": -1
        },
        "conditional_formatting": [],
        "aggregation": {
            "show_total": false,
            "show_average": false
        },
        "others": {
            "include_empty_children_rows": false
        },
        "quick_pivot": false,
        "sort": [
            {
                "column": 1,
                "sortOrder": false
            }
        ],
        "pop_settings": null
    },
    "format": {},
    "filters": [
        {
            "operator": "is",
            "values": [
                "primary"
            ],
            "modifier": null,
            "options": null,
            "path_hash": {
                "model_id": "public_anime_titles",
                "field_name": "title_type"
            },
            "aggregation": null,
            "label": "Adhoc filter",
            "dynamic_filter_id": null
        },
        {
            "operator": "top",
            "values": [
                5
            ],
            "modifier": {
                "logic": "standard",
                "aggregation": "max",
                "field_name": "tilte_length",
                "model_id": "public_anime_titles",
                "label": "Title length"
            },
            "options": null,
            "path_hash": {
                "model_id": "public_anime_titles",
                "field_name": "title"
            },
            "aggregation": null,
            "label": "Adhoc filter",
            "dynamic_filter_id": null
        }
    ],
    "adhoc_fields": []
}
```

Because of AniDB's limit on API call (multiple requests can get you banned
easily -- turns out that the limit is quite small; about 13-14 calls already got
me banned...), I'm going to just study the top 25 anime in terms of title
length.

Figure \@ref(fig:release-date) suggest that super long titles are more common in
the last decade than in the past. But the analysis is only based on top 25 anime
with the longest titles so it could benefit from more extensive study.

```aml
@viz
{
    "viz_type": "line_chart",
    "fields": {
        "x_axis": {
            "label": "Aired Date",
            "path_hash": {
                "field_name": "aired_date",
                "model_id": "public_myanimelist"
            },
            "type": "date",
            "uuid": "76f9730d-bfac-4c08-b386-8d0ada789f23",
            "transformation": "datetrunc year",
            "format": {
                "type": "date",
                "sub_type": "yyyy"
            },
            "aggregation": null
        },
        "series": {
            "path_hash": null,
            "format": null,
            "custom_label": null
        },
        "y_axes": [
            {
                "columns": [
                    {
                        "label": "Title Length",
                        "path_hash": {
                            "field_name": "title_length",
                            "model_id": "public_myanimelist"
                        },
                        "type": "auto",
                        "uuid": "bcc8b03c-2baa-4dd2-9cfc-773a43f25edb",
                        "aggregation": "max",
                        "format": {
                            "type": "number",
                            "format": {
                                "pattern": "inherited"
                            }
                        },
                        "color": "auto",
                        "series_settings": {
                            "series_hash": {},
                            "palette_id": -2,
                            "series_type": "auto"
                        },
                        "transformation": null
                    }
                ]
            }
        ]
    },
    "settings": {
        "x_axis": {
            "title": null
        },
        "legend": {
            "enabled": true,
            "alignment": "bottom"
        },
        "y_axes": [
            {
                "title": null,
                "align": "left",
                "scale_type": "linear",
                "min": null,
                "max": null,
                "show_data_label": false,
                "show_series_percentage": false,
                "stack_series": false,
                "stack_type": "normal",
                "show_stack_total": false,
                "show_group_total": false,
                "group_long_tail": false,
                "maximum_groups": 5
            }
        ],
        "misc": {
            "custom_color_list": [
                {}
            ],
            "pagination_size": 25,
            "show_row_number": true,
            "row_limit": -1
        },
        "quick_pivot": false,
        "sort": {},
        "pop_settings": null,
        "others": {
            "include_empty_children_rows": false
        }
    },
    "format": {},
    "filters": [
        {
            "operator": "not_null",
            "values": [],
            "modifier": null,
            "options": null,
            "path_hash": {
                "model_id": "public_myanimelist",
                "field_name": "aired_date"
            },
            "aggregation": null,
            "label": "Adhoc filter",
            "dynamic_filter_id": null
        }
    ],
    "adhoc_fields": []
}
;;
```
![](https://emitanaka.org/posts/2022-01-16-anime-titles/figures/release-date-1.png)
