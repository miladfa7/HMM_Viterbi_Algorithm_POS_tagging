# HMM_Viterbi_Algorithm_POS_tagging

Homework #3- HMM-based POS tagging

Acknowledgment: This homework is adapted from NLP Course at University of Colorado.


In this assignment you are to implement an HMM-based approach to POS tagging. Specifically, you are
to implement the Viterbi algorithm using a bigram tag/state model. As training data, I am providing a
POS-tagged section of the BERP corpus. Your systems will be evaluated against an unseen test set drawn
from the same corpus.

--------------------
* SAMPLE SENTENCE
i 	PRP
'd 	MD
like VB
french JJ
food NN
.    .

---------------------

* Training

The training data consists of around 15,000 POS-tagged sentences from the BERP corpus. The sentences
are arranged as one word-tag pair per line with a blank line between sentences, words and tags are tab-
separated. Contractions are split out into separate tokens with separate tags. An example is shown above.
You should assume that the tags that appear in the training data constitute all the tags that exist (no new
tags will appear in testing). On the other hand, new words may appear in the test set.

* Decoding

For decoding, your system will read in sentences from a file with the same format minus the tags. That
is one word per line ending with a period and blank line before the next sentence. As output you should
emit an appropriate tag for each word in the same format as the training data.

* Evaluation

To know if you're making progress on improving your system during development you need to have an
evaluation metric. You need to provide appropriate python code for evaluation that reports a simple
aggregate accuracy score. 
where berp-key.txt contains the word <tab> tag pairs for the gold standard tags and berp-out.txt contains
the system output to be evaluated.
  
* Assignment

To complete the assignment, you'll need to address the following problems:
Extract the required counts from the training data for the various probability estimates that are needed.
Deal will unknown words in some systematic way.
Do some form of smoothing (for the bigram transition probabilities).
Implement the Viterbi algorithm.
Provide an evaluation script that gives you, precision, recall, F-measure and
Accuracy.

* What to hand in
You will need to turn in your code, along with a short report describing what you did and all the choices
you made. You should include in your report how you assessed your system (training/dev) and how well
you believe it works. In addition, a test set is provided for you to run your system on shortly before the
due date. Include the output of your system on this test data in the same form as the training data.
