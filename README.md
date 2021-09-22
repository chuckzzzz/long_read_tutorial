**generate QC for a run of ONT long reads** 

`pycoQC -f {path_to_summary.txt} -o {name.html}`

options:

- -f input file
- -o output file

------------------------------------------------------------------------------------------------------

**Trim adapter sequences with porechop**

Use porechop to trim the adapter sequences. discard_middle option will also remove reads with internal adapter sequences 

`porechop -i {input_directory_of_fastq_file} -o {output_directory_of_trimmed_fastq_file} -{other_options}`

`porechop -i ~/Workspace/long_read_tutorial/data/course_data/precompiled/guppy_output/all_guppy.fastq  -o ~/Workspace/long_read_tutorial/data/course_data/practicals/trimming_practical/porechop/porechopped.fastq --discard_middle`

options

- -i input file
- -p output file
- --discard_middle remove reads that contains adapter sequences in the middle 

------------------------------------------------------------------------------------------------------

**Read trimming and filtering using NanoFilt**

options:

- -l remove all sequences shorter than {number} of nucleotides
- -headcrop remove the first {number} of nucleotides from all reads

`NanoFilt -l 500 --headcrop 10 < ~/Workspace/long_read_tutorial/data/course_data/practicals/trimming_practical/porechop/porechopped.fastq > /home/test/Workspace/long_read_tutorial/data/course_data/practicals/trimming_practical/nanofilt/nanofilt_trimmed.fastq`

----

**Use minimap2 to map fastq files to reference **

options:

- --MD outputs the MD flag which is needed by sniffles to identify structural variants 
- -a output in the sam format

`minimap2 --MD -a ~/Workspace/long_read_tutorial/data/course_data/precompiled/chr17.fas ~/Workspace/long_read_tutorial/data/course_data/precompiled/guppy_output/all_guppy_concat.fastq > mapped.sam`

----

**Use samtools to convert sam to bam and create corresponding index **

`samtools view -bS mapped.sam > mapped.bam`

`samtools sort -o mapped.sorted.bam mapped.bam`

`samtools index mapped.sorted.bam`

----

Use sniffles to identify structural variants 

options:

- -m: mapped reads file
- -v: output to vcf file

`sniffles -m mapped.sorted.bam -v variants.vcf` 

