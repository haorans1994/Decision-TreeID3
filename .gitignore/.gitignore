# -*- coding: UTF-8 -*-
from __future__ import division
import math
import string
def Datainput():
    f = open('C:\python\KDD.txt', 'r')
    lines = f.readlines()
    data = []
    for line in lines:
        l = line.split(',')[1:]
        data.append(l)
    d = data[50078:]
    attributes = ['duration','protocol_type','service','flag','src_bytes','dst_bytes','land','wrong_fragment','urgent','hot','num_failed_logins','logged_in','num_compromised',
    'root_shell','su_attempted','num_root','num_file_creations','num_shells','num_access_files','num_outbound_cmds','is_hot_login','is_guest_login','count','srv_count','serror_rate',
    'srv_serror_rate','rerror_rate','srv_rerror_rate','same_srv_rate','diff_srv_rate','srv_diff_host_rate','dst_host_count','dst_host_srv_count','dst_host_same_srv_rate','dst_host_diff_srv_rate',
    'dst_host_same_src_port_rate','dst_host_srv_diff_host_rate','dst_host_serror_rate','dst_host_srv_serror_rate','dst_host_rerror_rate','dst_host_srv_rerror_rate']
    return d,attributes



def calcentropy(data):
    totalnum = len(data)
    labelcount = {}
    for entity in data:
        currentlabel = entity[-1]
        if currentlabel not in labelcount.keys():
            labelcount[currentlabel] = 0
        labelcount[currentlabel] += 1
    entropy= 0.0
    for key in labelcount:
        p_key = labelcount[key] / totalnum
        entropy -=  p_key * math.log(p_key,2)
    return entropy



def splitdata(data,i,value):
    subdata = []
    for entity in data:
        if entity[i]==value:
            deletelabels = entity[:i]
            deletelabels.extend(entity[i+1:])
            subdata.append(deletelabels)
    return subdata



def bestsplitpoint(data):
    variablenum = len(data[0]) - 1
    entropy_parent = calcentropy(data)
    bestIG = 0.0
    bestlabel = -1
    for i in range(variablenum):
        variablelist = [example[i] for example in data]
        Val=set(variablelist)
        entropy_child = 0.0
        for value in Val:
            subdata = splitdata(data,i,value)
            prob = len(subdata) / len(data)
            entropy_child += prob * calcentropy(subdata)
        IG = entropy_parent - entropy_child
        if(IG>bestIG):
            bestIG = IG
            bestlabel = i
    return bestlabel



def createtree(data,labels):
    classlist = [entity[-1] for entity in data]
    if classlist.count(classlist[0]) == len(classlist):
        return classlist[0]
    if len(data[0]) == 1:
        return majority(classlist)
    besti = bestsplitpoint(data)
    bestlabel = labels[besti]
    tree = {bestlabel:{}}
    del(labels[besti])
    featvalue = [entity[besti] for entity in data]
    Vals = set(featvalue)
    for value in Vals:
        sublabels = labels[:]
        tree[bestlabel][value] = createtree(splitdata(data,besti,value),sublabels)
    return tree



def majority(classlist):
    classcount = {}
    for vote in classlist:
        if note not in classcount.keys():
            classcount[vote] = 0
        classcount[vote] += 1
    sortedclasscount = sorted(classcount.iteritems(),key = operator.itemgetter(1),reverse=True)
    return sortedclasscount



def classify(tree,label,testVec):
    firstStr = tree.keys()[0]
    secondDict = tree[firstStr]
    Index = label.index(firstStr)
    classlabel = ''
    for key in secondDict.keys():
        if testVec[Index] == key:
            if type(secondDict[key]).__name__=='dict':
                classlabel = classify(secondDict[key],label,testVec)
            else:
                classlabel = secondDict[key]
    return classlabel



def storeTree(inputtree,filename):
    import pickle
    fw = open(filename,'w')
    pickle.dump(inputtree,fw)
    fw.close()


def gettree(filename):
    import pickle
    fr = open(filename)
    return pickle.load(fr)



ff = open('C:\python\prediction.txt', 'r')
lines = ff.readlines()
predata = []
answer = []
for line in lines:
    l = line.split(',')[1:-1]
    predata.append(l)
    ll = line.split(',')[1:]
    answer.append(ll)







traindata,alllabels = Datainput()
tree = createtree(traindata,alllabels)
data1,labels1 = Datainput()

predict = []
for i in range(0,len(predata)):
    pp = classify(tree,labels1,predata[i])
    predict.append(pp)

count = 0
for i in range(0,len(predata)):
    if predict[i]==answer[i][-1]:
        count = count + 1

accuracy = count/len(predata)
print 'accuracy =', accuracy
