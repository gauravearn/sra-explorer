# genome_pangenome_sra-explorer
a native enumaltor for the sra explorer and it will explore the sra and will bring all the information and will generate the go to files for the analysis. You simply have to provide as many as much as posisble SRA accessions for the use of the same. It uses a regular expressions plus two associative arrays to store all the information. A python version and a rust crate is also present for the use. In python and rust i implemented a binary search approach so that it will indexed the query as a native graph node. 
```
#! /usr/bin/bash
# Universitat Potsdam
# Author Gaurav Sablok
# date: 2024-2-19
# a pure native enumaltor for the sra explorer by Phil
# it works with multiple and overcomes the limitations of the exploration
# it works on the command line and therefore can be invoked on any cluster or the instance
echo "there are two options available such as print or the links"
echo "if print selected it will print the results to the terminal"
echo "if links selected then it will write the wget files for the 
        corresponding SRA accessions"
read -r -p "do you want to generate the links or print:" option
if [[ "${option}" == "print"]]
then 
    declare -a ncbisra=()
    while true; do
        read -r -p "please provide the GSE numbers to search for the NCBI sra accession:" accession
        ncbisra+=("${accession}")
        if [[ "${accession}" == "" ]]; then
            echo "finished taking the SRA accessions"
            break
        fi
    done
    echo "processing the list of the accessions provided"
        for i in "${ncbisra[*]}"; do
            echo "${i}"
        done
    echo "processing the list of the sra accessions 
                        and generating the download counts"
    for i in "${ncbisra[*]}"; do
     echo "wget -F "${i}" -o "${i}".log"
    done
    for i in $(ls -l *.log); do
        echo "cat "${i}" grep "GSM[0-9][0-9][0-9][0-9][0-9][0-9]" \
                                                >"${i%.*}".selected.txt"
    done
    echo "generating the native scale information for the selected GSM"
    for i in "${i%.*}".selected.txt; do
        echo "grep ">GSM[0-9][0-9][0-9][0-9][0-9][0-9]" -o "{i}" | \
                                    cut -f 2 -d ">" >"${i%.*}".ids.txt"
    done
    echo "generating the SRX links for the selected GSM"
    for i in "${i%.*}".selected.txt; do
            echo "grep ">SRX[0-9][0-9][0-9][0-9][0-9][0-9]" -o "{i}" | \
                                    cut -f 2 -d ">" >"${i%.*}".idslinks.txt"
    done
    echo "generating the SRxlinks"
    echo "please check the provided links for the SRX"
        for i in $(grep ">SRX[0-9][0-9][0-9][0-9][0-9][0-9]" -o "{i}" | \
                  cut -f 2 -d ">" >"${i%.*}".idslinks.txt); do
         echo "https://www.ncbi.nlm.nih.gov/sra/"+"${i}"+[accn]
    done
    declare -a linkretriveal=()
    for i in $(grep ">SRX[0-9][0-9][0-9][0-9][0-9][0-9]" -o "{i}" | \
        cut -f 2 -d ">" >"${i%.*}".idslinks.txt); do
        linkretriveal+=("${i}")
        if [[ -z "${i}" ]]; then
            echo "finished making the indexed array for the links retrival"
        fi
    done
    for i in "${linksretrival[*]}"; do
        for j in $(grep ">SRX[0-9][0-9][0-9][0-9][0-9][0-9]" -o "{i}" | \
            cut -f 2 -d ">"); do
            echo "wget -F "${j}" -o "{j%.*}".SRX.log"
        done
    done
    for i in $(ls -l *.SRX.log); do
        for j in $(grep ">SRR[0-9][0-9][0-9][0-9][0-9][0-9]" -o "{i}" | \
                                                    cut -f 2 -d ">"); do
            echo "the links for the download of the respective 
                        fasta or the fastq files from the SRA are"
            echo "https://www.be-md.ncbi.nlm.nih.gov/Traces/sra-reads-be/fastq?"+"acc"+"="${j}"
            echo "https://www.be-md.ncbi.nlm.nih.gov/Traces/sra-reads-be/fasta?"+"acc"+"="${j}"
        done
    done
elif [[ ${option} == "links" ]]
then
    declare -a ncbisra=()
    while true; do
        read -r -p "please provide the GSE numbers to search for the NCBI sra accession:" accession
        ncbisra+=("${accession}")
        if [[ "${accession}" == "" ]]; then
            echo "finished taking the SRA accessions"
            break
        fi
    done
    echo "processing the list of the accessions provided"
    for i in "${ncbisra[*]}"; do
        echo "${i}"
    done
    echo "processing the list of the sra accessions 
                         and generating the download counts"
    for i in "${ncbisra[*]}"; do
         wget -F "${i}" -o "${i}".log
    done
    for i in $(ls -l *.log); do
        cat "${i}" grep "GSM[0-9][0-9][0-9][0-9][0-9][0-9]" \
                                             >"${i%.*}".selected.txt
    done
        echo "generating the native scale information for the selected GSM"
            for i in "${i%.*}".selected.txt; do
                grep ">GSM[0-9][0-9][0-9][0-9][0-9][0-9]" -o "{i}" |
                    cut -f 2 -d ">" >"${i%.*}".ids.txt
    done
        echo "generating the SRX links for the selected GSM"
    for i in "${i%.*}".selected.txt; do
        grep ">SRX[0-9][0-9][0-9][0-9][0-9][0-9]" -o "{i}" | \
                   cut -f 2 -d ">" >"${i%.*}".idslinks.txt
    done
        echo "generating the SRxlinks"
        echo "please check the provided links for the SRX"
    for i in $(grep ">SRX[0-9][0-9][0-9][0-9][0-9][0-9]" -o "{i}" | \
                     cut -f 2 -d ">" >"${i%.*}".idslinks.txt); do
    echo "https://www.ncbi.nlm.nih.gov/sra/"+"${i}"+[accn]
    done
    declare -a linkretriveal=()
    for i in $(grep ">SRX[0-9][0-9][0-9][0-9][0-9][0-9]" -o "{i}" | \
        cut -f 2 -d ">" >"${i%.*}".idslinks.txt); do
        linkretriveal+=("${i}")
     if [[ -z "${i}" ]]; then
            echo "finished making the indexed array for the links retrival"
        fi
    done
    for i in "${linksretrival[*]}"; do
        for j in $(grep ">SRX[0-9][0-9][0-9][0-9][0-9][0-9]" -o "{i}" | \
            cut -f 2 -d ">"); do
             wget -F "${j}" -o "{j%.*}".SRX.log
    done
    done
    for i in $(ls -l *.SRX.log); do
        for j in $(grep ">SRR[0-9][0-9][0-9][0-9][0-9][0-9]" -o "{i}" | \
                                                    cut -f 2 -d ">"); do
         echo "the links for the download of the respective 
                        fasta or the fastq files from the SRA are"
            "https://www.be-md.ncbi.nlm.nih.gov/Traces/sra-reads-be/fastq?"+"acc"+"="${j}"
             "https://www.be-md.ncbi.nlm.nih.gov/Traces/sra-reads-be/fasta?"+"acc"+"="${j}"
        done
    done
else 
                 echo "no option selected"
fi
```

