# BME205_HW4

If you don't know how to use markdown, [go to this website:](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet). Great! Now you can contribute to this file! :)

[And go here if you don't know how to use git:](http://rogerdudler.github.io/git-guide/)

## Hw4 requirements to get an 'A'

-Things that Karplus wrote in the assignment are in " "

-Things that Darrin wrote are not in " ", but aim to clarify what
 Karplus wrote

### Task list for Assignment 4, based on order of appearance on his site

1. "You will build stochastic models of these sequences as zero-order
and first-order Markov chains, and measure the information gain of the
first-order model over the zero-order model."

2. "so that 'A' and 'a' both stand for alanine." Make the sequences
all uppercase to avoid this problem.
    - [X] DTS seq.upper() in count_kmers() method

3. "You will do 2-fold cross-validation (training on each of the set
and testing on the other)." This is in reference to mark_f14_1.seqs
and mark_f14_2.seqs

4. Access the two above files using
"/soe/karplus/.html/bme205/f14/markov_files/ so that you don't need to
make your own copies.  (Note: the "f14" is correct---I'm reusing the
files from last year.)"

5. Use "tiny.seqs set for quick testing."
   - [X] **DTS** using this currently. Didn't modify at all yet

6. Use the special 'stop' and 'start' characters at the beginning and
end of a sequence to use in the HMM. "the start of a sequence (^) and
end of the sequence($)"
    - [X] **DTS** solved this using this line:
    `new_seq='^'*(k-1) +seq.upper() +'$'*(k-1)`
    This allows you to use the kmer-generating for-loop Karplus
    provides without doing any weird edge cases

7. It might be a good idea in your code to have separate parameters
for start and stop characters, and to allow start=Noneto mean that
k-mers that start before the first character of the sequence are not
counted (similarly for stop=None.
    - [ ] **DTS** Does this just mean that he wants us to exclude
      characters like '^^A', '^AC', and 'BO$' and kmers like that?
      (Only if start=Noneto of course)

8. Make sure that your count_kmers() method works when P_0 (aka when
k=0) "The simplest model we discussed in class is the
i.i.d. (independent, identically distributed) model, or zero-order
Markov model. It assumes that each character of the string comes from
the same background distribution P_0." *Karplus goes on to write* "To
estimate the background distribution, we need counts of all the
letters in the training set, plus the number of strings in the
training set (represented as the number of times "$" occurs)."
    - [ ] **DTS** Not sure - I would assume that using an 'if'
      statement for when k=1 is the best way to go
