# Machine Learning Labs

My submitted lab work for a university machine learning / data science course, done in
early 2022. Nine R Markdown files, one per lab, covering base R, OLS from scratch, the
perceptron and SVM, KNN, ggplot2, Rcpp, trees / bagging / random forests, `data.table`
wrangling, and probability estimation with logistic regression and forward stepwise
selection.

## Attribution — read this first

**The prose in these files is not mine. It is the instructor's.**

These labs were handed out as `.Rmd` templates from the class repository. Everything you
see in normal text — the problem statements, the explanations, the ROxygen function
specs (`@param`, `@return`), the scaffolding chunks that set up data or plot a result,
and the `#TO-DO` markers — was written by the instructor. My contribution is only:

- the R code I wrote inside the chunks, and
- the plain-text answers I typed under the prompts that asked for a written response
  (usually the lines beginning with `#`).

The course is Math 342W (with a parallel 650 masters section — several exercises are
marked as required for masters students and extra credit for undergraduates). Labs 7-9
use the instructor's own R package, [YARF](https://github.com/kapelner/YARF).

I am publishing this as a record of coursework, not as original work. If you want to see
code that is entirely mine, this is the wrong repository.

## What each lab covers

| Lab | Topic |
|-----|-------|
| 1 | Base R: vectors, factors, matrices, `apply`/`lapply`/`split`, sampling, missingness |
| 2 | `my_reverse`, `flip_matrix`, binary classification on iris, perceptron, SVM via `e1071`, 1-NN |
| 3 | `my_simple_ols` from scratch, estimation error vs. n, Galton height data, `my_ols` |
| 4 | Design matrix and hat matrix by hand, projections, SST = SSR + SSE |
| 5 | Diamonds: log transforms, nested model comparison A-F, in-sample vs. oos error, K-fold CV |
| 6 | ggplot2 on the GSS vocabulary dataset |
| 7 | Rcpp (`all_angles`, Fibonacci), trees, bagging, random forests, `mtry` tuning |
| 8 | Bagged OLS, RF imputation of missing data, `data.table` wrangling on `storms`, table joins |
| 9 | Bills/payments/discounts join, classification tree, asymmetric costs, ROC/AUC, forward stepwise |

## Known incomplete and non-running parts

Listing these because a reviewer will find them and should know they were known rather
than missed. A good number are exercises the assignment itself marked as extra credit or
as masters-only; the rest are things I ran out of time on.

**Empty exercises (function documented, body left as `#TO-DO`):**

- Lab 2, line 474 — `linear_svm_learning_algorithm`. The version above it (line 450) has
  my pseudocode in comments, which is what was required of 342W students; the real
  implementation below it is empty. The chunk that plots its line will therefore error.
- Lab 2, line 368 — maximum-margin perceptron (extra credit).
- Lab 2, line 567 — KNN with a `k` argument (masters / extra credit).
- Lab 3, line 298 — dataset with R^2 near 1 and arbitrarily high RMSE (masters / extra credit).
- Lab 4, line 58 — the X-perpendicular matrix (masters).
- Lab 5, line 324 — computing `s_e_s_F` over 200-observation slices. The `ggplot` call
  under it references `s_e_s_F`, so the chunk errors.
- Lab 7, lines 294, 372, 441 — bias-variance decomposition, and two plots of oob /
  bootstrap error by number of trees.
- Lab 8, lines 40, 46, 52 — punching holes in a matrix, building `Xmiss`, and the random
  forest imputation loop.
- Lab 8, lines 244, 250 — the two `storms` aggregation exercises.
- Lab 8, from line 508 onward ("Everything below here is due with lab 9") — error metrics,
  asymmetric costs, logistic regression, ROC, AUC, and the stepwise loop are all empty in
  Lab 8. Most of that material is answered in Lab 9 instead.
- Lab 9, line 169 — assigning asymmetric costs.

**Code that would not run as written:**

- Lab 2, `flip_matrix` — the parameter is `x` but the body refers to `X`. It only works if
  a matrix named `X` happens to exist in the global environment.
- Lab 2, second `nn_algorithm_predict` (line 545) — the spec asked me to add a distance
  argument `d`, but I never added it to the signature, and the body calls `Xstar` which is
  never defined. The simpler one-argument version above it (line 506) does work.
- Lab 3, `my_simple_ols` — divides by `n` taken from the global environment instead of
  `length(x)`. It gives correct answers only when the global `n` matches the input length.
  Later in the same lab a chunk runs `rm(list = ls())` and then calls the function again,
  which is why my own comment at line 280 says the RMSE and R^2 came out different than
  expected and I could not explain it. That is the reason.
- Lab 8, `random_bagged_ols` — `sample(p_se, ...)` is a typo for `p_seq`, and the function
  never fits or returns any models.
- Lab 9, the forward stepwise `repeat` loop — passes `type = "binomial"` where `glm` wants
  `family = "binomial"`, fits `yselect` against the training design matrix, and stores the
  chosen predictor index in `in_sample_brier_by_iteration` instead of a Brier score. The
  plot at the end is drawn from that, so it does not show what it claims to.

**Missing data and external dependencies:**

- Labs 8 and 9 read `bills_dataset/bills.csv.bz2`, `payments.csv.bz2` and
  `discounts.csv.bz2`. Those files were course-supplied and are not in this repository, so
  those sections cannot be re-run here.
- Lab 8, line 105 calls `source()` on a raw githubusercontent URL to pull in a `pmean`
  helper. That executes third-party code fetched over the network at knit time. I would not
  write it that way now — the right move is to vendor the one function or use
  `rowMeans(..., na.rm = TRUE)`.
- Labs 7-9 need a JDK, `rJava`, and the `YARF` package installed from GitHub.

## Running these

R with `pacman` installed. Each lab installs what it needs via `pacman::p_load`. Open a
`.Rmd` in RStudio and knit, or run chunk by chunk. Given the items above, several labs will
not knit end to end without edits.

## License

Deliberately none. The assignment text and scaffolding in these files are the instructor's
work and are not mine to license. The code I wrote inside the chunks is free for anyone to
read or reuse.
