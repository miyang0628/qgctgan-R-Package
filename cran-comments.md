## Submission

This is a new release (qgctgan 0.1.0), the first submission to CRAN.

## R CMD check results

0 errors | 0 warnings | 1 note

The note is the standard "New submission" note.

## Notes for the reviewer

* The optional `generator_ctgan()` adapter uses the 'reticulate' package to call
  the Python 'ctgan' library. 'reticulate' is in Suggests, the adapter guards it
  with `requireNamespace()`, and every example/test/vignette path that would
  require Python is wrapped in `\dontrun{}` / `eval = FALSE`. The package is
  fully functional, and the full test suite passes, with the default pure-R
  Gaussian generator and no Python installation.

* The ORCID iD in DESCRIPTION (0009-0004-1493-6691) is valid (ISO 7064 MOD 11-2
  checksum verified).

## Test environments

* local: Ubuntu 24.04, R 4.3.3 -- 0 errors | 0 warnings | 1 note
  (the note only reports that the optional Suggests package 'reticulate'
  was not installed in the local check environment).
* Before submission, also run on win-builder (devel + release) and R-hub, and
  confirm `R CMD check --as-cran` with internet access resolves the ORCID and
  URL checks.
