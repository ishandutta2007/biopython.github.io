---
title: Sequence Cleaner
permalink: wiki/Sequence_Cleaner
layout: wiki
---

Description
-----------

I wanna share my script using biopython to clean sequences up , you
should know analyzing poor data takes CPU time and interpreting the
results from poor data takes people time, so always is importat make a
preprocessing.

Let me call my script as “Sequence\_cleaner” and the big idea is to
remove duplicate sequences, remove sequence too short ( the user define
the minimum length) and remove sequences which has too much unknown
nucleotides (N) ( the user define the % of N is allows ) and in the end
the use can choose if he/she wanna have a file as output or print the
result.

Script
------

``` python
from Bio import SeqIO
 
def sequence_cleaner(fasta_file,min_length=0,por_n=100):
    #create our hash table to add the sequences
    sequences={}

    #Using the biopython fasta parse we can read our fasta input
    for seq_record in SeqIO.parse(fasta_file, "fasta"):
        #Take the current sequence
        sequence=str(seq_record.seq).upper()
        # Check if the current sequence and be in the hash table as a "clean" sequence according with the user parameters
        if (len(sequence)>=min_length and (float(sequence.count("N"))/float(len(sequence)))*100<=por_n):
            # If the sequence passed in the test "is It clean?" and It isnt in the hash table , the sequence and Its id gonna be in the hash table
            if sequence not in sequences:
                sequences[sequence]=seq_record.id
            # if It is in the hash table , We just gonna take its ID and concact with the Id of other sequence that was exaclty the same.
            else:
                sequences[sequence]+="_"+seq_record.id

            
    #Write the clean sequences
                
    #Create a file in the same directory where you ran this script
    output_file=open("clear_"+fasta_file,"w+")
    #Just Read the Hash Table and write on the file as a fasta format
    for sequence in sequences:
            output_file.write(">"+sequences[sequence]+"\n"+sequence+"\n")
    output_file.close()



#sequence_cleaner("Aip_coral.fasta",10,10)

"You should call the function 'sequence_cleaner', there are 3 basics parameters:
"                                               #1st: your fasta file 
"                                               #2nd: the user define the minimum length (default value 0 ( means  you dont care to minimum length)
"                                               #3rd: the user define the % of N is allows (default value 100 ( means  you dont care to 'N' in your sequences))
"FYI If You dont care to the 2nd and the 3rd parameters you just gonna remove the duplicate sequences

"
```