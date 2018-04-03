This page is a place to put requested features or changes to Red (not Red/System, we'll add  a new page for that if needed). It exists because we found more and more WISH tickets showing up in the issue tracking system, which is not it's main purpose. We also know that the REP process may be a bit heavy for every request, but we need to find a balance. Just having an idea, or wanting something to be different, without thinking it through and providing examples and use cases isn't very helpful to the team, and reduces the chances of a wish being granted. 

The plan is to migrate existing WISH tickets to this wiki page, and close them in the issue tracker. They can be reopened later if it makes sense. If you have a new wish, add it here, including an entry in the table of contents to help others find existing wishes. Flesh them out. Help us to help you by taking time to consider the details of your request, and possible consequences. If you're new to Red, and experienced Reducers express resistance to your ideas, don't take it personally. Red is a well designed, deeply thought out language. Not perfect, but there are no obvious, glaring problems in its design. There are choices you may not agree with, but it's sound and there are reasons for things working the way they are.



## Table of Contents

1. [Template](#template)
2. [](#)
3. [](#)
4. [](#)
5. [](#)
6. [](#)
7. [](#)
8. [](#)
9. [](#)

# Template

Submitted by *name* on *date*




# Retain issue!-as-string when not conflicting with keyword! type 

Kaj-de-Vos commented on Jan 9, 2013

From AltME:

My brain came up with a solution for issue! while I was sleeping. It's a notational problem, so how about having both issue! and keyword! start with a # but when the next character is alphabetic, it's a keyword, and otherwise, it's an issue!.

This is consistent with both issue notation in American English and preprocessor keywords in C-like languages.

It's an easy rule for the lexer, and a relatively easy rule for humans, that is intuitively clear.

It optimises keyword use for speed, while preventing memory leaking into the symbol table for almost all issue notations.

Many issues will not start with an alphabet character, but when they do, most cases will be transparent. In other cases, only little code needs to be added to handle them as keyword!.


