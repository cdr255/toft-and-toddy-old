---
layout: post
title:  "Character Details"
date:   2014-12-21 22:13:48
categories: writing downloads code
---

I play a lot of [Tabletop Roleplaying Games][ttrpgs] in my spare time.

In doing that, I have come up with some common questions one needs to
ask (and, more importantly, answer!) when creating a character- if
only so that, when something happens while playing, the character can
react as they would.

I'd made myself (and whoever wants a copy) a
[Document][char-details-doc] to fill out about each new character, but
that was large and clunky, requiring a word processor and a steady
hand (to prevent erasing the fields' text as You were editing Your
responses). I looked into [PDFs][pdfs], but they are slow, big, and
most importantly creating them is not well supported on
[GNU/Linux][linux], which is my Operating System of choice.

I also use [GNU/Emacs][emacs] exclusively as my text editor. And it
was _there_ that I found my most recent solution:
[forms-mode][forms-mode].

With a little bit of [ELISP][elisp] code, I was able to draft up a
system which stores all of the information in plaintext, and lets me
use my [favorite editor][emacs] to maintain them. It prints easily,
provided You keep the text hard-wrapped below 80 columns, and takes up
__WELL__ less than 1 MB of space on my HDD- which cannot be said for a
_single_ character detail sheet of the [Document][char-details-doc]
variety.

Below is the code, if You'd like to see it to come up with Your own. I
also have it available to [download][download], if You want to run the
same one I use. I prototype all major characters using it, though I do
update it from time to time to reflect the info I currently use.

Until next time!

{% highlight lisp linenos %}
;; character-details.form - Flesh Out Characters, v1.0     -*- forms -*-
     
     ;; This provides a form for the Character Detail Sheet, v1.0
     
     ;; Set the name of the data file.
     (setq forms-file "characters.dat")
     
     ;; Use forms-enumerate to set field names and number thereof.
     (setq forms-number-of-fields
           (forms-enumerate
            '(char-name
	      char-alias               
              char-race                    
              char-sex                     
              char-age                 
              char-height           
              char-weight
	      char-body
              char-hair
              char-eyes
              char-skin
              char-notable
              char-costume
	      char-region
	      char-town
	      char-father
	      char-mother
	      char-sibling
	      char-relatives
	      char-childhood
	      char-crimes
	      char-means
	      char-means-explain
	      char-ends
	      char-ends-explain
	      char-worship
	      char-worship-explain
	      char-marry
	      char-myers-briggs
	      char-myers-briggs-ie
	      char-myers-briggs-sn
	      char-myers-briggs-ft
	      char-myers-briggs-jp
	      char-myers-briggs-dom
	      char-myers-briggs-aux
	      char-myers-briggs-ter
	      char-myers-briggs-inf
	      char-ipip-neo-ext-gen
	      char-ipip-neo-ext-fri
	      char-ipip-neo-ext-gre
	      char-ipip-neo-ext-ass
	      char-ipip-neo-ext-act
	      char-ipip-neo-ext-exc
	      char-ipip-neo-ext-che
	      char-ipip-neo-agr-gen
	      char-ipip-neo-agr-tru
	      char-ipip-neo-agr-mor
	      char-ipip-neo-agr-alt
	      char-ipip-neo-agr-coo
	      char-ipip-neo-agr-mod
	      char-ipip-neo-agr-sym
	      char-ipip-neo-con-gen
	      char-ipip-neo-con-eff
	      char-ipip-neo-con-ord
	      char-ipip-neo-con-dut
	      char-ipip-neo-con-ach
	      char-ipip-neo-con-dis
	      char-ipip-neo-con-cau
	      char-ipip-neo-neu-gen
	      char-ipip-neo-neu-anx
	      char-ipip-neo-neu-ang
	      char-ipip-neo-neu-dep
	      char-ipip-neo-neu-con
	      char-ipip-neo-neu-imm
	      char-ipip-neo-neu-vul
	      char-ipip-neo-opn-gen
	      char-ipip-neo-opn-ima
	      char-ipip-neo-opn-art
	      char-ipip-neo-opn-emo
	      char-ipip-neo-opn-adv
	      char-ipip-neo-opn-int
	      char-ipip-neo-opn-lib
	      char-sex-pref
	      char-strangers
	      char-role
	      char-prejudices
	      char-convo
	      char-fave-food
	      char-hate-food
	      char-fave-drink
	      char-hate-drink
	      char-fave-color
	      char-hate-color
	      char-fave-music
	      char-hate-music
	      char-fave-symbol
	      char-hate-symbol
	      char-fave-time
	      char-hate-time
	      char-non-adv
	      char-self-desc
	      char-group-like
	      char-group-left
	      char-strengths
	      char-know-best
	      char-one-question
	      char-bad-time
	      char-one-thing
)))
     
     ;; The following functions are used by this form for layout purposes.
     ;;
     (defun arch-tocol (target &optional fill)
       "Produces a string to skip to column TARGET.
     Prepends newline if needed.
     The optional FILL should be a character, used to fill to the column."
       (if (null fill)
           (setq fill ? ))
       (if (< target (current-column))
           (concat "\n" (make-string target fill))
         (make-string (- target (current-column)) fill)))
     ;;
     (defun arch-rj (target field &optional fill)
       "Produces a string to skip to column TARGET\
      minus the width of field FIELD.
     Prepends newline if needed.
     The optional FILL should be a character,
     used to fill to the column."
       (arch-tocol (- target (length (nth field forms-fields))) fill))
     
     ;; Record filters.
     ;;
     (defun new-record-filter (the-record)
       "Form a new record with some defaults."
       (aset the-record char-name (user-full-name))
       (aset the-record char-age (current-time-string))
       the-record)                           ; return it
     (setq forms-new-record-filter 'new-record-filter)
     
     ;; The format list.
     (setq forms-format-list
          (list
            "====== Character Detail Sheet ======\n\n"
            char-name
            " - "                    char-alias
	    "\n\n"
            '(arch-tocol 70 ?-)
            "\n== Personal Effects ==\n\n"
            "Race: "                 char-race
            " Sex: "                char-sex
	    " Age: "                char-age
            "\n\n"
            ;'(arch-tocol 40)
            "Height: "               char-height
	    " Weight: "               char-weight
	    "\n\nBody Type: "            char-body
            "\n\n"
            ;'(arch-rj 73 10)
            "Hair: "                 char-hair
            "\n\nEyes: "                 char-eyes
            "\n\nSkin: "                 char-skin
            "\n"
            "\n\nNotable Physical Characteristics:\n"
	    char-notable
            "\n\nDefault Costume:\n"
            char-costume
	    "\n\n"
	    '(arch-tocol 70 ?-)
            "\n== Social History ==\n\n"
	    "Home Region: "           char-region
	    " Home Town: "            char-town
	    "\n\n"
	    "Childhood Events:\n"
	    char-childhood
	    "\n\nConvicted of the Following Crimes:\n"
	    char-crimes
	    "\n\n-- Family Ties --\n"
	    "Father:\n"
	    char-father
	    "\n\nMother:\n"
	    char-mother
	    "\n\nSibling:\n"
	    char-sibling
	    "\n\nOther Relatives:\n"
	    char-relatives
	    "\n\n"
	    '(arch-tocol 70 ?-)
	    "\n== Philosopical and Religious Leanings ==\n\n"
	    "Alignment(means): "             char-means
	    "\nInterpretation:\n"
	    char-means-explain
	    "\n\nAlignment(ends): "            char-ends
	    "\nInterpretation:\n"
	    char-ends-explain
	    "\n\n"
	    "Worships: "           char-worship
	    "\nWhy?\n"
	    char-worship-explain
	    "\n\n"
	    '(arch-tocol 70 ?-)
	    "\n== Interpersonal Relationships ==\n\n"
	    "-- Myers-Briggs: --\n\n"
	    "Short: "                                  char-myers-briggs
	    "\nDevotions: "
	    "\n     Introversion/Extraversion: "       char-myers-briggs-ie
	    "\n     Sensing/iNtuition: "               char-myers-briggs-sn
	    "\n     Feeling/Thinking: "                char-myers-briggs-ft
	    "\n     Judging/Perceiving: "              char-myers-briggs-jp
	    "\nFunctions: "
	    "\n     Dominant: "                        char-myers-briggs-dom
	    "\n     Auxillary: "                       char-myers-briggs-aux
	    "\n     Tertiary: "                        char-myers-briggs-ter
	    "\n     Inferior: "                        char-myers-briggs-inf
	    "\n"
	    "\n-- IPIP-NEO-PI: --\n\n"
	    "Extraversion: "                char-ipip-neo-ext-gen
	    "%\n     Friendliness: "         char-ipip-neo-ext-fri
	    "%\n     Gregariousness: "       char-ipip-neo-ext-gre
	    "%\n     Assertiveness: "        char-ipip-neo-ext-ass
	    "%\n     Activity Level: "       char-ipip-neo-ext-act
	    "%\n     Excitement-Seeking: "   char-ipip-neo-ext-exc
	    "%\n     Cheerfulness: "         char-ipip-neo-ext-che
	    "%\nAgreeableness: "             char-ipip-neo-agr-gen
	    "%\n     Trust: "                char-ipip-neo-agr-tru
	    "%\n     Morality: "             char-ipip-neo-agr-mor
	    "%\n     Altruism: "             char-ipip-neo-agr-alt
	    "%\n     Cooperation: "          char-ipip-neo-agr-coo
	    "%\n     Modesty: "              char-ipip-neo-agr-mod
	    "%\n     Sympathy: "             char-ipip-neo-agr-sym
	    "%\nConscientiousness: "         char-ipip-neo-con-gen
	    "%\n     Self-Efficacy: "        char-ipip-neo-con-eff
	    "%\n     Orderliness: "          char-ipip-neo-con-ord
	    "%\n     Dutifulness: "          char-ipip-neo-con-dut
	    "%\n     Achievement-Striving: " char-ipip-neo-con-ach
	    "%\n     Self-Discipline: "      char-ipip-neo-con-dis
	    "%\n     Cautiousness: "         char-ipip-neo-con-cau
	    "%\nNeuroticism: "               char-ipip-neo-neu-gen
	    "%\n     Anxiety: "              char-ipip-neo-neu-anx
	    "%\n     Anger: "                char-ipip-neo-neu-ang
	    "%\n     Depression: "           char-ipip-neo-neu-dep
	    "%\n     Self-Consciousness: "   char-ipip-neo-neu-con
	    "%\n     Immoderation: "         char-ipip-neo-neu-imm
	    "%\n     Vulnerability: "        char-ipip-neo-neu-vul
	    "%\nOpenness to Experience: "    char-ipip-neo-opn-gen
	    "%\n     Imagination: "          char-ipip-neo-opn-ima
	    "%\n     Artistic Interests: "   char-ipip-neo-opn-art
	    "%\n     Emotionality: "         char-ipip-neo-opn-emo
	    "%\n     Adventurousness: "      char-ipip-neo-opn-adv
	    "%\n     Intellect: "            char-ipip-neo-opn-int
	    "%\n     Liberalism: "           char-ipip-neo-opn-lib
	    "%\n\n-- Dealings with Others --"
	    "\n\nMarital Status:\n"
            char-marry
	    "\n\nSexual Orientation:\n"           
	    char-sex-pref
	    "\n\nConsideration of Strangers:\n"
	    char-strangers
	    "\n\nRole (in a group):\n"
	    char-role
	    "\n\nPrejudices:\n"
	    char-prejudices
	    "\n\nConversation Style\n"
	    char-convo
	    "\n\n"
	    '(arch-tocol 70 ?-)
	    "\n== Petty Preferences ==\n\n"
	    "Favorite Food: "           char-fave-food
	    " Hated Food: "             char-hate-food
	    "\n\nFavorite Drink: "        char-fave-drink
	    " Hated Drink: "            char-hate-drink
	    "\n\nFavorite Color: "        char-fave-color
	    " Hated Color: "            char-hate-color
	    "\n\nFavorite Music: "        char-fave-music
	    " Hated Music: "            char-hate-music
	    "\n\nFavorite Symbol: "       char-fave-symbol
	    " Hated Symbol: "           char-hate-symbol
	    "\n\nFavorite Time of Day: "  char-fave-time
	    " Hated Time of Day: "      char-hate-time
	    "\n\n"
	    '(arch-tocol 70 ?-)
	    "\n== Interview Questionaire ==\n\n"
	    "When You are not adventuring, what are You doing?\n"
	    char-non-adv
	    "\n\nTell me about Yourself.\n"
	    char-self-desc
	    "\n\nWhat do You look for in a group?\n"
	    char-group-like
	    "\n\nWhy did You leave Your last group?\n"
	    char-group-left
	    "\n\nWhat are Your stengths?\n"
	    char-strengths
	    "\n\nWhat do You know best?\n"
	    char-know-best
	    "\n\nWhat is one thing You always ask people?\n"
	    char-one-question
	    "\n\nTell me about a time when things went poorly.\n"
	    char-bad-time
	    "\n\nFinally, do You have anything You want people to know?\n"
	    char-one-thing
	    "\n\n"
	    '(arch-tocol 70 ?-)
	    

          ))

{% endhighlight %}

[ttrpgs]: http://en.wikipedia.org/wiki/Role-playing_game "Like D&D, Pathfinder, GURPS..."
[char-details-doc]: http://cdr255.com/misc.html "This is the one to use if You do not use GNU/Emacs."
[pdfs]: http://en.wikipedia.org/wiki/Portable_Document_Format "PDFs are one of the most troublesome file formats I have ever dealt with."
[linux]: http://www.slackware.com "I use Slackware unashamedly, on every PC I own."
[emacs]: http://www.gnu.org/software/emacs/ "The first thing I install on a workstation I am forced to use. If I had chosen to use it, it was already installed."
[forms-mode]: http://www.gnu.org/software/emacs/manual/forms.html "Forms mode may look a bit dated, but it is easily my favorite new toy."
[elisp]: http://en.wikipedia.org/wiki/Emacs_Lisp "LISP is a language I have a love and hate relationship with. In this case, though, I love it."
[download]: /assets/character-details.form "If You want to edit this version, simply say 'no' when it asks You about processing the LISP code."
