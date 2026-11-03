# Introduction
In this project, we implement the Burrows–Wheeler transform (BWT) and use it to find all exact matches of a query in a reference sequence. We first build the transform by sorting all rotations of the reference, then compute it from a suffix array. We use character counts and the LF mapping to search the query from right to left, narrowing the range of possible matches before using the suffix array to recover their starting positions.

We also apply run-length encoding to the transformed string and decode it to recover the BWT. This project demonstrates how aligners such as Bowtie and BWA use FM indexes to search for exact matches.

# Pseudocode
```
FUNCTION BWT(string)
    Add "$" if the string does not already contain it
    Generate and sort all cyclic rotations
    RETURN the last character of each sorted rotation joined into a string

FUNCTION suffix_array(string)
    Append "$" if needed
    List all suffixes in starting-position order
    Sort the suffixes lexicographically
    RETURN each sorted suffix's index in the original list

FUNCTION BWT_from_suffix_array(text, suffix_positions)
    Append "$" if needed
    For each suffix position, take the character immediately before it
    For position 0, wrap around to the last character
    RETURN the selected characters joined into a string

FUNCTION cal_count(string)
    Count the frequency of each character
    Walk through the characters in sorted order, keeping a running total
    RETURN the number of characters that sort before each character

FUNCTION cal_occur(bwt_string)
    Scan the BWT from left to right, keeping a running count for each character
    At each position, record the counts through that position
    RETURN the occurrence table

FUNCTION update_range(lower, upper, count, occur, character)
    Find where the character's block starts in the first column using count
    Use occur to count copies of the character strictly before each boundary
    # LF mapping preserves the order of each character's occurrences
    Add each occurrence count to the start of the character's block
    RETURN the new lower and upper boundaries

FUNCTION find_match(query, reference)
    Append "$" if needed
    Build the suffix array and BWT from the reference
    Build the count table from the reference and the occurrence table from the BWT
    Start with the full suffix array as the candidate range

    FOR each query character, from right to left
        IF the character is absent from the count table
            RETURN an empty list
        Narrow the range using update_range
        IF lower >= upper
            RETURN an empty list

    RETURN the sorted reference positions in the range [lower, upper)

FUNCTION run_length_encode(bwt_string)
    Group consecutive copies of each character into runs
    Write each run as the character followed by its length
    RETURN the encoded string

FUNCTION run_length_decode(encoded)
    Read each character and all the digits that follow it
    Repeat the character according to that count
    RETURN the reconstructed BWT string

FUNCTION main(reference, query)
    Compute the direct BWT and the BWT from the suffix array
    Build the count and occurrence tables from the suffix-array BWT
    Find the query's exact match positions in the reference
    Encode the BWT and decode the encoded string
    Print the suffix-array BWT, the match positions, and the encoded string
```

# Successes
Description of the team's learning points

# Struggles
Description of the stumbling blocks the team experienced

# Personal Reflections
## Group Leader
Group leader's reflection on the project

## Other member
Other members' reflections on the project

# Generative AI Appendix
As per the syllabus
