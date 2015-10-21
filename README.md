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

9. The next simplest model was a Markov chain, in which the
probability of each character depends only on the preceding
character. To estimate the conditional probabilities, we'll need the
counts for all pairs of letters in the training set, including initial
pairs (like "^M") and ending pairs (like "K$"). Note that "^$" is a
possible pair (what one would get for the empty string).
    - [ ] **DTS** is this when `k=1`? If so I don't think my line
      above to add `^` and `$` to an empty string will work. Maybe
      add a conditional case where if `string=None`, just return `^$`
      as the empty string containing only the start and stop character

10. In general, for estimating a k-th order Markov chain, we need
counts of all (k+1)-mers in the training data.
    - [X] **DTS** I think I got this working in the for loop in
      `count_kmers()` with this line: `for start in
      range(len(new_seq)-k+1):`

### First program: count-kmers

11. Write a program called "count-kmers" that reads a FASTA file from
stdin and outputs a table of counts to stdout. **ONLY ADD A CHECKMARK
WHEN 100% FINISHED**

12. The output should have two tokens on each line: the k-mer followed
by the count of that k-mer.
   - [ ] **DTS** I'm guessing he just wants it `\t` separated again. That was easy.
   
   ```
   def print_kmers(counter):
    for value in sorted(counter):
        print(value,'\t',counter[value])
   ```

13. Initial and final k-mers should be symmetrically included, that
is, if the sequence is ABDEF and you are counting 3-mers, the counts
should be:
    - [X] **DTS** This works for me since I fulfilled #6 and #11
      above. Can't find a case where it breaks so far, but I haven't
      done much testing

      ```
      ^^A 1
      ^AB 1
      ABD 1
      BDE 1
      DEF 1
      EF$ 1
      F$$ 1
      ```

14. Note: case should be ignored in the input sequence, so that
"abDE", "ABDE", and "abde" are all counted as the same 4-mer.
    - [X] **DTS** This is redundant with #6 above. Already took care
      of this with `string.upper()`

15. "In one [approach to generaing kmers], you process the file a line
at a time (to distinguish id lines), then process the sequence lines a
character at a time, using something like `kmer = kmer[0:-1]+letter.`"
*Karplus then writes* "Both approaches are useful. The first one does
not require large amounts of RAM when you get very long sequences
(like whole plant chromosomes),"
    - [X] **DTS** I had no idea what he was doing here so I didn't use
      this method

16. In the other approach, you can define a generator function in
Python to read a FASTA file one sequence at a time, then extract the
kmers from the sequence with something like:
    - [X] **DTS** I used this approach and it works well if you prepend
     with `(k-1)*^` and `(k-1)*$`
     
     ```
     for (fasta_id,comment,seq) in read_fasta(genome):
     for start in range(len(seq)-k):
     counts[seq[start:start+k]] += 1
     ```

17. count-kmers.py must have this option: "--order(or -o) a required
argument that gives the order of the model (hence --order=0should
count 1-mers)"
      - [X] **DTS** Completed this, to get aliases for options just do
        this: `parser.add_argument("--order", "-o")`

18. --alphabet(or -a)
    1. which specifies which letters of the sequence are to be used
      (for example, to count only standard amino acids, use --
      alphabetACDEFGHIKLMNPQRSTVWY).
        - [X] **DTS** haven't done this yet
    2. "All characters in sequences that are not in the alphabet should be ignored."
       - [ ] **DTS** need to test this
    3. "Note: you may want to use the "set" type to represent the alphabet internally."
       - [ ] **DTS** I have no idea how to implement this.
       - [ ] **DTS** Doesn't this need to be in count_kmers?
    4. "The alphabet should default to ASCII letters if not specified."
       - [X] **DTS** Did this already in parse_arguments with `const=string.ascii_letters`
    5. "Note: you want case conversion to be done before determining whether a letter is in the alphabet, so that users don't have to provide both lower-case and upper-case copies of the alphabet."

19. "The command-line option parsing should be done with the "argparse" module."

20. "The actual counting of k-mers should be done by a function you
define in a separate module (Markov.py or markov.py)."

21. "The counts are most easily kept and returned in a
"collections.Counter" object, which provides a very convenient way to
do all sorts of counting."
   - [ ] **DTS** already using collections.Counter

22. "If the k-mer counting function is passed a file-like object (like
sys.stdin) as an argument, then you can easily reuse it on other files"
   - [ ] **DTS** Need to do this still

23. "Furthermore, if you use `for line in input_stream:` to read the
input, then you can do testing by passing in a list of lines"

   ```
   >>> dummy_in= '>seq1\nABCDEF\n>seq2\nAC\nde\nFG\n>seq3\n'
   >>> count=get_counts(dummy_in.splitlines(True),
                        2, alphabet="ABCDEFGHIJK", start='^', stop='$')
   ```

24. 