# tommy
See if we can amplify additional vivid barcodes from either pol 2 (gene expression library) or pol 3 (“gRNA fraction”). Because I have had undue difficulty sequencing the pol 2 (~1.2kb) from the polyA transcript, below I have worked with the ~300bp shorter fragment that comes from the capture-seq.

I ran 
This searches through my 4 fastq files and searches each line to see if we can find the CBC from the aggregated ViViD set within the MiSeq data.

The fastq’s are in the following folder of s3://insitro-sequencing-archive/  :
file_paths_miseq = [ #here you can choose your FASTQs
'231204_M06199_0347_000000000-LBC2F/fastqs/AACTAGCA_S1_L001_R1_001.fastq.gz',	'231204_M06199_0347_000000000-LBC2F/fastqs/ATGGACTA_S4_L001_R1_001.fastq.gz',
'231204_M06199_0347_000000000-LBC2F/fastqs/CAATGCTC_S3_L001_R1_001.fastq.gz',
'231204_M06199_0347_000000000-LBC2F/fastqs/GACATCCG_S2_L001_R1_001.fastq.gz'
	]

Short fragment snapgene file 
Long fragment snapgene file
Samples
AACTAGCA: ViViD-n=12_D10_DMSO_flow
ATGGACTA: ViViD-n=12_D10_bort_SOP
CAATGCTC: ViViD-n=12_D10_DMSO_SOP
GACATCCG: ViViD-n=12_D10_bort_flow


How to run
Run through cell that starts with “#check to see if the barcodes that I have from MiSeq are within the CBC from 10X aggregated file”

Output:
Matching barcode in first 16 bases: TCTCCGAGTCGCACGT
Matching barcode in first 16 bases: ACGATGTGTTTGATCG
Matching barcode in first 16 bases: TTTCAGTGTTCCGGTG
Total lines checked in 231204_M06199_0347_000000000-LBC2F/fastqs/AACTAGCA_S1_L001_R1_001.fastq.gz: 3064225
Total matching lines in first 16 bases in 231204_M06199_0347_000000000-LBC2F/fastqs/AACTAGCA_S1_L001_R1_001.fastq.gz: 391
Matching barcode in first 16 bases: AATCGTGGTGGTCCCA
Matching barcode in first 16 bases: AGTGCCGCACTTCAAG
Matching barcode in first 16 bases: AATCGTGGTGGTCCCA
Total lines checked in 231204_M06199_0347_000000000-LBC2F/fastqs/ATGGACTA_S4_L001_R1_001.fastq.gz: 3101797
Total matching lines in first 16 bases in 231204_M06199_0347_000000000-LBC2F/fastqs/ATGGACTA_S4_L001_R1_001.fastq.gz: 556
Matching barcode in first 16 bases: AGCCAATGTCTTCAAG
Matching barcode in first 16 bases: GTTGTCCGTAACACCT
Matching barcode in first 16 bases: GTTGTCCGTAACACCT
Total lines checked in 231204_M06199_0347_000000000-LBC2F/fastqs/CAATGCTC_S3_L001_R1_001.fastq.gz: 2644282
Total matching lines in first 16 bases in 231204_M06199_0347_000000000-LBC2F/fastqs/CAATGCTC_S3_L001_R1_001.fastq.gz: 2303
Matching barcode in first 16 bases: TGTTGAGGTAAGCTCT
Matching barcode in first 16 bases: TGTTGAGGTAAGCTCT
Matching barcode in first 16 bases: TGTTGAGGTAAGCTCT
Total lines checked in 231204_M06199_0347_000000000-LBC2F/fastqs/GACATCCG_S2_L001_R1_001.fastq.gz: 3113452
Total matching lines in first 16 bases in 231204_M06199_0347_000000000-LBC2F/fastqs/GACATCCG_S2_L001_R1_001.fastq.gz: 180


Interpretation
There are between 180 and 2300 cell barcode matches found within my MiSeq data. This might be encouraging, if not for the below caveat of UMI collapsing. 

I know that R2 doesn’t read anything (all NNNN) where it is supposed to be my ViViD barcode. This is unusual and I don’t know why (especially considering all P5 and P7 looks clean)



Areas for code improvement
I am not doing any “UMI collapsing”, so there is a fairly non-zero chance that the lines that contain my CBC might be coming from a trivial number of UMIs (last 12 bases of the 28bp read in MiSeq). This ought to be fairly straightforward with some additional filtering steps
As Anna and I discussed, I am using a filtered/aggregated “filtered_feature_bc_matrix/barcodes.tsv.gz”, which is probably downsampled and should be compared to the full size / across each barcodes.tsv file per sample
If you really want to do a deep dive, I ran another MiSeq where my vivid barcodes are actually the “index 1” and index 2 should go to the p5 side (though this had single P5 index bc it was one sample), and read 2 is fake
Data (not extracted) at s3://insitro-sequencing-archive/231214_M06199_0353_000000000-LBRP4/
These would be the “indexes”

CS9CJPiALS-n1 | pb-VIVID-hNIL | 6/20/23
GCTATCCCCACG
LVC0006 | kiVCP-R155Chet | CL.D1 | pb-VIVID-hNIL | 6/20/23
ATGACCTCTTCG
Cins917 | corr_C9ORF72_het | CL.19 | pb-VIVID-hNIL | 6/20/23
GGACGCACCATT
CS6UC9iALS-n1 | pb-VIVID-hNIL | 6/20/23
ACGTCAACTGCT
CW70251 | kiTARDBP-M337Vhet | CL.H3 | pb-VIVID-hNIL | 6/20/23
CGTAATTTTGTA
LVC0006 | kiTARDBP-M337Vhet | CL.D8 | pb-VIVID-hNIL | 05/16/23
CGTAATTTTGTA
LVC0005 | kiTARDBP-M337Vhet | CL.F8 | pb-VIVID-hNIL | 6/20/23
TTAGCCCTCGAT
LVC0005 | kiVCP-R155Chet | CL.D7 | pb-VIVID-hNIL | 6/20/23
CCTGTCGCTATC
Cins128 | mock_WT | CL.A3 | pb-VIVID-hNIL | 6/20/23
TGCGCCTTACTC
Cins913 | corr_C9ORF72_het | CL.3 | pb-VIVID-hNIL | 05/16/23
TCGGAAGCAAAC
Cins089 | mock_WT | CL.B1 | pb-VIVID-hNIL | 6/20/23
AAGGCAATTTAC
Cins129 | mock_wt | CL.B2 | pb-VIVID-hNIL | 6/20/23
GTTAATCCACCG


