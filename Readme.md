# Count the unique k-mers in a set of sequences

This program calculates the number of distinct k-mers for each sequence record in a fasta file and divides it by the total number of k-mers in that record. Provided the number of possible distinct k-mers (4^k) is at least as large as the sequence length, this gives a measure of how diverse the k-mer composition of the sequence is. Repetitive sequences or sequences with restricted nucleotide composition, for example, will tend to have low values.

The program, which is writtern in Rust, is based on [Needletail](https://github.com/onecodex/needletail)'s fast FASTA parser, making it efficient for large read sets.

## Usage

`unique-kmers --file <fasta file> --klength <k-mer length> --out <output file>`

- Input: A multi-entry fasta file with nucleotide sequences. The file may be gzip-ed.
- Output: A text file where each row corresponds to one sequence record, giving the number of ratio of the number of distinct k-mers over the total number of k-mers in the sequence (the output is currently given as a 64-bit float).
- k-mer length `k` should be set so that 4^k > the length of the longest sequence in the set, but note that smaller values will be more computationally efficient. We use `k = 8` for HiFi-reads. Values > 255 are not permitted.

### Notes:

Note that k-mers will **not** be canonicalised. That is, each k-mer and its reverse complement will be treated as distinct observations. Since the purpose of this program is not to report **which** k-mers were observed in a given sequence, and all k-mers within a given record are on the same strand, there is no need to merge observations. If you are interested in recording which k-mers are contained in a set of sequences, you might try [kmer-counter](https://github.com/CobiontID/kmer-counter) instead.
  
## Installation
- Install Rust (see https://www.rust-lang.org/tools/install)
- Download the source code from this repository
- Navigate to the directory containing `Cargo.toml`
- Run `cargo build --release`. The resulting binary will be located in `./target/release/unique-kmers` unless otherwise specified.

## Citation
If you use this k-mer counter in your work, please cite: https://doi.org/10.1093/g3journal/jkae187

Bibtex:
> @article{Weber_2024, title={Disentangling cobionts and contamination in long-read genomic data using sequence composition}, volume={14}, url={http://dx.doi.org/10.1093/g3journal/jkae187}, DOI={10.1093/g3journal/jkae187}, number={11}, journal={G3: Genes, Genomes, Genetics}, author={Weber, Claudia C}, year={2024} }

