# Detector performance and LLM-likeness shift

## Detector performance on held-out e-SNLI
- Main TF-IDF + Logistic Regression: F1=0.9246, AUROC=0.9733
- Style-only control: F1=0.6922, AUROC=0.7723
- Length-matched: F1=0.9094, AUROC=0.9611, matched train human/LLM=5274/5274, matched eval human/LLM=5004/5004

## LLM-likeness score distributions

### Main detector
| group     |     mean |    median |   pct_above_threshold |     n |
|:----------|---------:|----------:|----------------------:|------:|
| new_human | 0.337633 | 0.340463  |               4.01581 | 12650 |
| old_human | 0.183558 | 0.0782369 |               2.3     |  8000 |
| old_llm   | 0.836526 | 0.855621  |              65.3875  |  8000 |

### Style-only control
| group     |     mean |   median |   pct_above_threshold |     n |
|:----------|---------:|---------:|----------------------:|------:|
| new_human | 0.522896 | 0.452562 |               14.83   | 12650 |
| old_human | 0.381413 | 0.331638 |                4.0875 |  8000 |
| old_llm   | 0.628284 | 0.660738 |               35.8125 |  8000 |

### Length-matched control
| group     |     mean |   median |   pct_above_threshold |    n |
|:----------|---------:|---------:|----------------------:|-----:|
| new_human | 0.382696 | 0.399842 |               3.29736 | 5004 |
| old_human | 0.218202 | 0.110323 |               2.8777  | 5004 |
| old_llm   | 0.796594 | 0.820791 |              58.6131  | 5004 |

## Qualitative audit sample
| explanation                                                                            | group     |   llm_likeness | bucket     |
|:---------------------------------------------------------------------------------------|:----------|---------------:|:-----------|
| The United States                                                                      | new_human |       0.998017 | high_score |
| The regulation was in accordance with the orthodox structure according to the document | new_human |       0.986988 | high_score |
| The answer fits with the article                                                       | new_human |       0.982929 | high_score |
| The answer fits with the article                                                       | new_human |       0.982929 | high_score |
| The government wanted to terrorize the citizens and opposition in Aleppo.              | new_human |       0.98223  | high_score |
| The names are given in the article                                                     | new_human |       0.975353 | high_score |
| The practice has died out with the exception of Barrett in this school district.       | new_human |       0.969477 | high_score |
| The response is relevant and consistent with the facts in the document.                | new_human |       0.965711 | high_score |
| The response is relevant and consistent with the facts in the document.                | new_human |       0.965711 | high_score |
| The answer misrepresents the facts in the article                                      | new_human |       0.960164 | high_score |
| The answer fits with the text                                                          | new_human |       0.957565 | high_score |
| The answer fits with the text                                                          | new_human |       0.957565 | high_score |
| The answer fits with the text                                                          | new_human |       0.957565 | high_score |
| The answer fits with the text                                                          | new_human |       0.957565 | high_score |
| The answer fits with the text                                                          | new_human |       0.957565 | high_score |
| The answer fits with the text                                                          | new_human |       0.957565 | high_score |
| The answer fits with the text                                                          | new_human |       0.957565 | high_score |
| The answer fits with the text                                                          | new_human |       0.957565 | high_score |
| The answer fits with the text                                                          | new_human |       0.957565 | high_score |
| The answer fits with the text                                                          | new_human |       0.957565 | high_score |

## Overlap caveat
- Pair overlap is zero across old-human train/eval and old-LLM train/eval, but exact explanation text overlap is not zero. Human exact overlap train/eval=33, human cleaned overlap train/eval=35, LLM cleaned overlap train/eval=18, cross-class cleaned overlap eval=1.

## Interpretation note
These results estimate whether newer human explanation text is stylistically closer to the old synthetic-LLM reference than to the old human baseline. They do not prove that newer annotators used LLMs.