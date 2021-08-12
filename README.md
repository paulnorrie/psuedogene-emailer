# psuedogene-emailer
Develop a service to identify customers with a specified psuedogene and email them.

We have a database of our customers' DNA sequences.  To keep things simple for this exercise, we’ll simplify our customers' biology. Treat a DNA sequence as a string of characters.  Each character represents a nucleotide which is one in the set of: “A”, “T”, “C”, “G”, “U”.  Our customers aren’t particularly sophisticated beings so the maximum length of their DNA strings is 232 characters.  We have over 10 million customers.  Like you, our customers have genes that allow us to create the proteins that make us up.  Genes only take up a small proportion of DNA sequences.  One of the other elements in a DNA sequence are psuedogenes. A pseudogene is like a gene but it doesn’t end up creating any proteins. Sometimes these psuedogenes turn out to be real genes, or otherwise important.

From time to time, we want to be able to send out bulk emails to customers who have a specified pseudogene that can be found in their DNA sequence.  The emails have will be drafted be drafted by our marketing department in a content management system.  

## Your mission
You need to create some working code for this service and submit it.  Your solution must search through the database for any customers that have a given psuedogene and email them the given content.  At our next discussion, we will want to go through your solution with you to understand why you made the decisions you did.

## Requirements and additional information
1. The relational database has the following, very simple data structure where customers can have 1 or more ordered DNA sequences:
```
┌───────────────────────────────┐     ┌──────────────────────────────────┐
│Customers                      │     │DNA                               │
├───────────────────────────────┤     ├──────────────────────────────────┤
│PK id          INTEGER NOT NULL│1─┐  │ PK id           INTEGER NOT NULL │
│   email_addr  CHAR(100)       │  └─*│ FK customer_id  INTEGER NOT NULL │
│   first_name  CHAR(50)        │     │    sequence     CHAR(2^16)       │
└───────────────────────────────┘     │    order        INTEGER NOT NULL |
                                      └──────────────────────────────────┘
```
Random DNA data can be created at https://www.bioinformatics.org/sms2/random_dna.html

2. The psuedocode for identifying a psuedogene is:
```
Given the following inputs:

dna: A string of characters, that are expected to be in the set of “A”, “T”, “C”, or “G” (the possible nucleotide bases), representing a DNA sequence.
start_codon: A string of 3 characters representing the 3 nucleotide basesthat identify the start of a psuedogene. The characters are expected to be in the set of “A”, “T”, “C”, or “G”
stop_codon: of 3 nucleotide bases that identifies where the psuedogene ends.The characters are expected to be in the set of “A”, “T”, “C”, or “G”

A psuedogene is found by searching dna for start_codon.  If start_codon is found, then find stop_codon at least three characters after start_codon.  If the stop_codon is found, then return all characters from the beginning of the start_codon to the beginning of the stop_codon (i.e. include the start_codon but not the stop_codon).
```

3. You should send emails using SMTP to an external SMTP service.  You should use a Mock SMTP server like [MailHog](https://github.com/mailhog/MailHog) or [Mailslurper](https://www.mailslurper.com/)

1. The content of the email is provided to you as a string. If the string contains the substring "{first_name}" then it needs to be replaced by the customers first name.

3. Your code should be able to be invoked over HTTP using curl

1. It is important that the every customer, who should be sent an email, is sent one, and emails are only sent to those customers.  The service should coded to be reliable, i.e. the mean-time between failure should be high.  You can assume the database is replicated (hot standby) and the SMTP service has a backup server (e.g. smtp.example.com and smtp2.example.com.

1. This service could be called at any time and possibly called while it is already busy with a previous request.

1. Your submission should include a README.MD file containing:
  - Instructions to build and deploy your solution, including any dependancies you require. 
  - Any assumptions you have made.
  - How you suggest the solution is deployed to handle a large amount of requests and be highly available.
  - Documentation to developers on how to use the service
  - Any suggested improvements to make it more maintainable, performant, or otherwise "better"




