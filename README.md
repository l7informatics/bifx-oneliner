Bioinformatics One Liners
=========================

This repository collects and explains the `bash` one-liners I tweeted using the [#BioinformaticsOneLiner hash tag](https://twitter.com/search?f=realtime&q=%23bioinformaticsoneliner&src=typd). The idea behind this project is to demonstrate how to perform many basic bioinformatics tasks using widely available shell utilities like `awk` and `sed`.

**Note**: Although many (most?) bioinformaticians use some flavor of Linux for their work, I've tried to make these one-liners as POSIX-ly correct as possible, which basically means avoiding GNU-specific extensions like the ~ "operator" in GNU `sed`. POSIX correctness makes some of these one-liners slightly more verbose than their GNU counterparts, but doing so makes them usable on systems like OS X, where the default versions of utilities like `awk` and `sed` lack the GNU extensions.
