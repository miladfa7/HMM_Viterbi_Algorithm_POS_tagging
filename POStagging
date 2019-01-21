import math
UNKOWN = '<u>'
START = '<s>'
UnigramCount = {}
TagTagCount = {}
TagWordCount = {}
DictonaryTagsToEachWord = {}
ViterbiSpace = {}
BackPointer = {}
transition = {}
emission = {}


def Probability_TagTag(i, j):
    # C(Tag,Tag)/C(Tags)
    return math.log10(float(TagTagCount.get((i+"/"+ j), 1)) / (UnigramCount[i]))

def Probability_TagWord(i, j):
    # C(Tag,Words)/C(Tags)
    return math.log10(float(TagWordCount.get((i+"/"+ j), 1)) / (UnigramCount[i]))

#def matchkey(i,j):
    #return i + '/' + j
    
def loadDataTrain(filename):
    with open(filename, 'r') as inputFile:
        tags, words = [START], [START]
        for line in inputFile:
            #print(line) more    JJR
            items = line.split()
            if items != []:
                (word, tag) = tuple(items)
                tags.append(tag)
                words.append(word)
        tags.append('&&')
        words.append('&&')
    return words, tags

def loadDataTest(filename):
    with open(filename, 'r') as inputFile:
        tags, words = [START], [START]
        for line in inputFile:
            items = line.split()
            if items != []:
                (word, tag) = tuple(items)
                tags.append(tag)
                words.append(word)
        tags.append('&&')
        words.append('&&')
    return words

    
def Viterbi(test):
    observation = loadDataTest(test)
    ViterbiSpace['0/&&'] = 1.0
    BackPointer['0/&&'] = None
    for tag in DictonaryTagsToEachWord[observation[1]]:

        
        ViterbiSpace['1'+"/"+ tag] = Probability_TagTag(START, tag) + Probability_TagWord(tag, observation[1])
        BackPointer['1'+"/"+ tag] = START
    
    for n in range(2, len(observation)):
        for tn in DictonaryTagsToEachWord.get(observation[n], DictonaryTagsToEachWord[UNKOWN]):
            vj = (str(n)+"/"+ tn)
            
            for ti in DictonaryTagsToEachWord.get(observation[n - 1], DictonaryTagsToEachWord[UNKOWN]):
                vi = str(n - 1)+"/"+ ti
               
                tag_tag = ti+"/"+ tn
                
                #print(tt) --> VB/PRP
                tw = tn+"/"+ observation[n]
                #print(tw)   NNS/bucks PRP/you
                if tag_tag not in transition:
                    transition[tag_tag] = Probability_TagTag(ti, tn)
                if tw not in emission:
                    emission[tw] = Probability_TagWord(tn, observation[n])
                    

                candidate = ViterbiSpace[vi] + transition[tag_tag] + emission[tw]
                
                if candidate > ViterbiSpace.get(vj, float('-inf')):
                    ViterbiSpace[vj] = candidate
                    BackPointer[vj] = ti

    prev = '&&'
    result = []
    for i in range(len(observation) - 1, 0, -1):
        
        if len(observation[i]) > 0:

            if '.' in observation[i]:

                result.append(observation[i])
            else:

                result.append("%s\t%s" % (observation[i], tag))
                
        if '.' in observation[i - 1]:
            result.append("")

        tag = BackPointer[(str(i)+"/"+ prev)]

        prev = tag
    f = open('Data/OutputResult.txt', 'r+')
    f.truncate()
    for r in reversed(result):
        with open('Data/OutputResult.txt', 'a') as the_file:
            the_file.write(r + '\n')
    words,tegs = loadDataTrain('Data/berp-key.txt')
    evaluationScript(BackPointer, words, tegs)
    

def HMM(filename):
    (words, tags) = loadDataTrain(filename)
    DictonaryTagsToEachWord[UNKOWN] = []
    
    for i in range(0, len(words)):

        tag_word = tags[i]+"/"+ words[i]

        if (tags[i] not in DictonaryTagsToEachWord[UNKOWN]):
            DictonaryTagsToEachWord[UNKOWN].append(tags[i])

        if TagWordCount.get(tag_word, 0) == 0:
            if words[i] not in DictonaryTagsToEachWord: 
                DictonaryTagsToEachWord[words[i]] = []

            DictonaryTagsToEachWord[words[i]].append(tags[i])

        UnigramCount[tags[i]] = UnigramCount.get(tags[i], 0) + 1
        UnigramCount[words[i]] = UnigramCount.get(words[i], 0) + 1
        TagWordCount[tag_word] = TagWordCount.get(tag_word, 0) + 1
        tag_2 = tags[i - 1]+"/"+ tags[i]
        TagTagCount[tag_2] = TagTagCount.get(tag_2, 0) + 1



def evaluationScript(back, observation, gold):
    
    result = [START]
    predict = ['&&']
    prev = predict[0]
    #print(prev) &&
    detect = 0
    new = 0
    allDetect = 0
    allNew = 0
    for i in range(len(observation) - 1, 0, -1):
        
        if observation[i] != '&&':
            if observation[i] in UnigramCount:
                allDetect += 1
                if predict[0] == gold[i]:
                    detect += 1
            else:
                allNew += 1
                if predict[0] == gold[i]:
                    new += 1

        tag = back[(str(i)+"/"+prev)]
        result.append("%s %s" % (observation[i], tag))
        predict.insert(0, tag)
        prev = tag

    precision = float((detect) / (detect + allDetect - detect)) * 100
    recall = float((detect) / (detect + allNew - new)) * 100
    f_measure = float((2 * precision * recall) / (precision + recall))
    accuracy = float(detect + new) / (allDetect + allNew) * 100
    
    print("\n")
    print("Precision: %.3g%%" % precision)
    print("Recall: %.3g%%" % recall)
    print("F_measure: %.3g%%" % f_measure)
    print("Accuracy: %.3g%%" % accuracy)


if __name__ == "__main__":
    
    HMM('Data/berp-POS-train.txt')
    Viterbi('Data/berp-key.txt')
