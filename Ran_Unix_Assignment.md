# **Unix_Assignment**
Unix Assignment 1 : Inspection of Fang_et_al_genotypes.txt and snp_positions.txt. Finally, the data in both files were processed per specification of the assigment. 


## **File Inspeciton**
This section describes knowing both files, contents, size, file type and clearly anything that can give a clue as to how the the data in the files can be processed. 


### **Inspection of fang_et_al_genotype.txt and snp_position.txt  with the codes below**

```
$ cd assignments
```

```
$ ls -l
```

```
$ du -h fang_et_al_genotypes.txt snp_position.txt
```

```
$ file fang_et_al_genotypes.txt snp_position.txt
```

```
$  wc snp_position.txt fang_et_al_genotypes.txt
```

```
$ file fang_et_al_genotypes.txt snp_position.txt
'''

```
$  wc fang_et_al_genotypes.txt snp_position.txt

```
$  head fang_et_al_genotypes.txt snp_position.txt
```

```
$ head -n 1 fang_et_al_genotypes.txt snp_position.txt
```

```
$ grep -v "^#" fang_et_al_genotypes.txt | awk -F "\t" '{print NF; exit}' fang_et_al_genotypes.txt
```

```
$ grep -v "^#" snp_position.txt | awk -F "\t" '{print NF; exit}' snp_position.txt
```

```
$ tail -n +2 snp_position.txt | cut -f1-2 | column -t
```

```
$ cut -f 3 fang_et_al_genotypes.txt |sort| uniq -c
```

### **Results of inspecting both files**

Following the order of the codes, I changed directory to access the files names fang_et_al_genotype and snp_position.txt. The fang_et_al_genotypes.txt and snp_position.txt files are 6.7M and 49K in size respectively. Also both files are ASCII text files with the fang_et_al_genotype have very long lines. With the 'wc' command on the files, fang_et_al_genotype.txt was known to have 2783 lines, 2744038 words and 11051939 bits of characters while the snp_position.txt file was seen to have 984 lines, 113198 words and 82763 bits of characters. The head commands used helped understand that all files had line as headers but the headers were not iniated by the "harsh symbol". The fang_et_al_genotype.txt and snp_position.txt files had 986 and 15 columns respectively. For snp_position.txt, the file was found to be not well sorted using the 'tail -n +2' command to inspect from line 2 to the end by observing the first and second columns. The last code for the inspection helped understand the number of lines a specific group of maize ccured in the fang_et_al_genotype.txt file. 


## **File processing**
This section describes how both files were processed to achieve a single file for further analysiz to be conducted. Following the assignment instructions, 44 files were generated with the following codes for Maize and Teosinte.

### **File Processing of fang_et_al_genotype.txt and snp_position.txt  with the codes below**

### **For Maize**

```
$ grep -E "(Group|ZMMIL|ZMMLR|ZMMMR)" fang_et_al_genotypes.txt | cut --complement -f 2-3 > tempo_maize_genotypes.txt
```

```
$ awk -f transpose.awk tempo_maize_genotypes.txt > tempo_transposed_maize_genotypes.txt
```

```
$ cut -f 1,3,4 snp_position.txt > tempo_position.txt
```

```
$ head  tempo_positions.txt | column -t 
```

```
$ grep -v "^SNP_ID" tempo_position.txt | sort -k1,1 > tempo_sorted_position.txt
```

```
$ head tempo_sorted_position.txt
```

```
$ grep -v "^Sample" tempo_transposed_maize_genotypes.txt | sort -k1,1 > tempo_sorted_maize_genotypes.txt
```

```
$ head -n 1 tempo_transposed_maize_genotypes.txt > tempo_maize_header.txt
```

```
$ head -n 1 tempo_positions.txt > tempo_position_header.txt
```

```
$ awk -F "\t" '{print NF; exit}' tempo_maize_header.txt
```

```
$ awk -F "\t" '{print NF; exit}' tempo_position_header.txt
```

```
$ cat tempo_maize_header.txt tempo_sorted_maize_genotypes.txt > temp_sorted_maize_all_complete.txt
```

```
$ awk -F "\t" '{print NF; exit}' tempo_position_header.txt
```

```
$ awk -F "\t" '{print NF; exit}' tempo_sorted_position.txt
```

```
$ cat tempo_position_header.txt tempo_sorted_position.txt > tempo_sorted_positions_all_complete.txt
```

```
$ awk -F "\t" '{print NF; exit}' tempo_sorted_positions_all_complete.txt
```

```
$ wc -l tempo_sorted_positions_all_complete.txt
```

```
$ awk -F "\t" '{print NF; exit}' tempo_sorted_positions_all_complete.txt
```

```
$ wc -l tempo_sorted_maize_all_complete.txt
```

```
$ awk -F "\t" '{print NF; exit}' tempo_sorted_maize_all_complete.txt
```

```
$ join -1 1 -2 1 -t $'\t' --header tempo_sorted_positions_all_complete.txt tempo_sorted_maize_all_complete.txt > tempo_merged_maize_all_complete.txt
```

```
$ grep -E '(Chromosome|unknown)' tempo_merged_maize_all_complete.txt | head -n 2
```

```
$ grep -E '(Chromosome|unknown)' tempo_merged_maize_all_complete.txt > maize_unknown_snps_positions.txt
```

```
$ grep -E '(Chromosome|multiple)' tempo_merged_maize_all_complete.txt > maize_multiple_snps_positions.txt
```

```
$ grep -v -E "(unknown|multiple)" tempo_merged_maize_all_complete.txt > tempo_merged_maize_chromosomes.txt
```

```
$ head -n 1 tempo_merged_maize_chromosomes.txt > tempo_headers_merged_maize.txt
```

```
$ sed 's/\?\/\?/\?/g' tempo_merged_maize_chromosomes.txt | sort -k2,2n -k3,3n | awk -F '\t' '{print $0 > "tempo_maize_increasing_chr_"$2".txt"}'
```

```
$ for i in $(seq 1 10); do cat tempo_headers_merged_maize.txt tempo_maize_increasing_chr_$i.txt > "maize_increasing_chr_${i}.txt"; done
```

```
$ sed 's/?\/?/-/g' tempo_merged_maize_chromosomes.txt | sort -k2,2n -k3,3nr | awk -F '\t' '{print $0 > "tempo_maize_decreasing_chr_"$2".txt"}'
```

```
$ for i in $(seq 1 10); do cat tempo_headers_merged_maize.txt tempo_maize_decreasing_chr_$i.txt > "maize_decreasing_chr_"$i".txt"; done
```


### **Results of Processing both files for Maize groups**

1. Creation of a tempo_maize_genotypes.txt from fang_et_al_genotypes.txt by using the grep command line, piped with the cut column option. The tempo_maize_genotypes.txt was then transposed to create tempo_transposed_maize_genotype.txt



2. Columns 1, 3 and 4 for the snp_position.txt were extracted out for creation of tempo_position.txt. A head command with tab spacing of columns was used to content as a verification step. 



3. The tempo_position.txt file was then sorted using column 1 as the point after grepping everything in the file aside the having the phrase "SNP_ID" with the result used in creating tempo_sorted_position.txt file.



4. The tempo_maize_transposed_genotypes.txt file was then sorted using column 1 as the point after grepping everything in the file aside the having the phrase "Sample". The result was sent to the output file tittled; tempo_sorted_maize_genotypes.txt 



5. Headers were created using the head -n 1 command on the tempo_transposed_maize_genotypes.txt to the new file 'temp_maize_header.txt' and same was done for the 'tempo_positions.txt' also to create 'temp_position_header.txt'. 



6. The number of columns of the transposed maize header and the snp_position files were examined and seen to be the 1574 and 3 using the awk command on both tempo_maize_header.txt and tempo_position_header.txt respectively.



7. A combined temp_sorted_maize_complete.txt file was created by cat command on both tempo_maize_header.txt and tempo_sorted_maize_genotypes.txt having 1574 columns in total.



8. tempo_position_header.txt and tempo_sorted_position.txt were both checked for column numbers and finally created a merge of both files with the cat command as tempo_sorted_positions_all_complete.txt



9. After checking the number of lines and columns of the complete files create for genotypes and snps, a join command was used to create a "tempo_merged_maize_all_complete.txt" file of 984 lines with the "tempo_sorted_positions_all_complete.txt" and "tempo_sorted_maize_all_complete.txt"



10. The join command was used to create a new file "tempo_merged_maize_all_complete.txt" from joining the two complete files after sorting them. 



11. The next subsequent codes with grep command were used to grep lines with Chromosome or unknown positions to create a new file "maize_unknown_snps_positions.txt". Grepping here extracted the header and all lines with the unknow for maize unknow snps positions.



12. Same grep command was used again in creating the maize file with mutiple chromosomes as maize_multiple_snps_positions.txt



13. Next, the file for only chromosomes were created with the multiple and unknow chromosome snps left out after greping. the file created with the code was tempo_merged_maize_chromosomes.txt



14. A first line for the file tempo_merged_maize_chromosomes.txt was extrated for a header file for the processed files so far. 



15. Finally, the sed command along with a loop designed were used to create both descending and ascending files of the processed maize genotype and snp positions. sed helped to replace the "?/?" to create the ascending files with "?" while "?/?" was replaced by "-" in the descending files. 


### **For Teosinte**

```
$ grep -E "(Group|ZMPBA|ZMPIL|ZMPJA)" fang_et_al_genotypes.txt | cut --complement -f 2-3 > tempo_teosinte_genotypes.txt
```

```
$ awk -f transpose.awk tempo_teosinte_genotypes.txt > tempo_transposed_teosinte_genotypes.txt
```

```
$ grep -v "^Sample" tempo_transposed_teosinte_genotypes.txt | sort -k1,1 > tempo_sorted_teosinte_genotypes.txt
```

```
$ head -n 1 tempo_transposed_teosinte_genotypes.txt > tempo_teosinte_header.txt
```

```
$ awk -F "\t" '{print NF; exit}' tempo_teosinte_header.txt
```

```
$ awk -F "\t" '{print NF; exit}' tempo_position_header.txt
```

```
$ cat tempo_teosinte_header.txt tempo_sorted_teosinte_genotypes.txt > tempo_sorted_teosinte_all_complete.txt
```

```
$ awk -F "\t" '{print NF; exit}' tempo_position_header.txt
```

```
$ awk -F "\t" '{print NF; exit}' tempo_sorted_position.txt
```

```
$ wc -l tempo_sorted_positions_all_complete.txt
```

```
$ wc -l tempo_sorted_teosinte_all_complete.txt
```

```
$ awk -F "\t" '{print NF; exit}' tempo_sorted_positions_all_complete.txt
```

```
$ awk -F "\t" '{print NF; exit}' tempo_sorted_teosinte_all_complete.txt
```

```
$ join -1 1 -2 1 -t $'\t' --header tempo_sorted_positions_all_complete.txt tempo_sorted_teosinte_all_complete.txt > tempo_merged_teosinte_all_complete.txt
```

```
$ grep -E '(Chromosome|unknown)' tempo_merged_teosinte_all_complete.txt | head -n 2
```

```
$ grep -E '(Chromosome|unknown)' tempo_merged_teosinte_all_complete.txt > teosinte_unknown_snps_positions.txt
```

```
$ grep -E '(Chromosome|multiple)' tempo_merged_teosinte_all_complete.txt > teosinte_multiple_snps_positions.txt
```

```
$ grep -v -E "(unknown|multiple)" tempo_merged_teosinte_all_complete.txt > tempo_merged_teosinte_chromosomes.txt
```

```
$ head -n 1 tempo_merged_teosinte_chromosomes.txt > tempo_headers_merged_teosinte.txt
```

```
$ sed 's/\?\/\?/\?/g' tempo_merged_teosinte_chromosomes.txt | sort -k2,2n -k3,3n | awk -F '\t' '{print $0 > "tempo_teosinte_increasing_chr_"$2".txt"}'
```

```
$ for i in $(seq 1 10); do cat tempo_headers_merged_teosinte.txt tempo_teosinte_increasing_chr_$i.txt > "teosinte_increasing_chr_${i}.txt"; done
```

```
$ sed 's/?\/?/-/g' tempo_merged_teosinte_chromosomes.txt | sort -k2,2n -k3,3nr | awk -F '\t' '{print $0 > "tempo_teosinte_decreasing_chr_"$2".txt"}'
```

```
$ for i in $(seq 1 10); do cat tempo_headers_merged_teosinte.txt tempo_teosinte_decreasing_chr_$i.txt > "teosinte_decreasing_chr_"$i".txt"; done
```


### **Results of inspecting both files for Teosinte groups**

1. Creation of a tempo_teosinte_genotypes.txt from fang_et_al_genotypes.txt by using the grep command line, piped with the cut column option. The tempo_teosinte_genotypes.txt was then transposed to create tempo_transposed_teosinte_genotype.txt



2. The tempo_teosinte_transposed_genotypes.txt file was then sorted using column 1 as the point after grepping everything in the file aside the having the phrase "Sample". The result was sent to the output file tittled; tempo_sorted_teosinte_genotypes.txt 



3. Headers were created using the head -n 1 command on the tempo_transposed_teosinte_genotypes.txt to the new file 'tempo_teosinte_header.txt' 



4. The number of columns of the transposed teosinte header was examined and seen to be 976 using the awk command on tempo_teosinte_header.txt



5. A combined temp_sorted_teosinte_complete.txt file was created by cat command on both tempo_teosinte_header.txt and tempo_sorted_teosinte_genotypes.txt having 976 columns in total.



6. tempo_position_header.txt and tempo_sorted_position.txt were both checked for column numbers and finally created a merge of both files with the cat command as tempo_sorted_positions_all_complete.txt



7. After checking the number of lines and columns of the complete files created for genotypes and SNPs, a join command was used to create a "tempo_merged_teosinte_all_complete.txt" file of 984 lines with the "tempo_sorted_positions_all_complete.txt" and "tempo_sorted_teosinte_all_complete.txt"



8. The join command was used to create a new file "tempo_merged_teosinte_all_complete.txt" from joining the two complete files after sorting them. 



9. The next subsequent codes with grep command were used to grep lines with Chromosome or unknown positions to create a new file "teosinte_unknown_snps_positions.txt". Grepping here extracted the header and all lines with the unknow for teosinte unknown snps positions.



10. The same grep command was used again in creating the teosinte file with multiple chromosomes as teosinte_multiple_snps_positions.txt



11. Next, the file for only chromosomes was created with the multiple and unknown chromosome snps left out after grepping. the file created with the code was tempo_merged_teosinte_chromosomes.txt



12. A first line for the file tempo_merged_teosinte_chromosomes.txt was extracted for a header file for the processed files so far. 


13. Finally, the sed command along with a loop command designed were used to create both descending and ascending files of the processed teosinte genotype and snp positions. sed helped to replace the "?/?" to create the ascending files with "?" while "?/?" was replaced by "-" in the descending files.
