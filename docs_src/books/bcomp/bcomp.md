---
title: Building a Compiler
last_edited: 2025-01-27
---

My personal notes on the book "A Construção de um Compilador" by Setzer&Melo (in Portuguese). The book is [hosted on the Internet Archive](https://archive.org/details/a-construcao-de-um-compilador-r1.2), and is also available on the [author's page](https://www.ime.usp.br/~vwsetzer/). It is a fundamental work for those who wish to understand the theory and practice of compiler construction. Published in 1989, the book remains current and approaches the subject in a didactic way, covering essential concepts, from lexical analysis to object code generation. It is an excellent reference for students and professionals in the field of computer science, offering a detailed and practical view of developer development.


## Parser extension

* PINTO, T. T. S. GGLL: um gerador de analisadores sintáticos para gramáticas gráficas LL(1). 2014. Universidade de São Paulo, 2014. DOI: 10.11606/D.45.2014.tde-23012015-075452. Available at: http://www.teses.usp.br/teses/disponiveis/45/45134/tde-23012015-075452/.

Abstract: This work focuses on the development of a top-down parser generator for LL(1) grammars with graphical input of the grammar, as well as a comparison of this generator with other generators in use in the market. As a result, a fully functional generator was obtained, and it was shown how it is superior to other parsers. Implementation details are described and a user manual for the system implemented in Java, independent of programming environments, was prepared.

Sources:

* <https://github.com/tassotirap/GGLL.UI>
* <https://github.com/tassotirap/GGLL.Core>


## Revision history

* r1.2b - optimization and conversion to PDF/A-2B using: `$ ocrmypdf --skip-text --tesseract-timeout=0 --jbig2-lossy --optimize=3 in.pdf out.pdf`
* r1.2a - the Initial View Navigation Tab has been changed from Page Only to Bookmarks Panel
* r1.2 - added Bookmarks
* r1.1 - OCRed
* r1.0 - original document scanned
  

## References

* AHO, A. V.; ULLMAN, J. D. The Theory of Parsing, Translation, and Compiling. Later Printing edition. Englewood Cliffs, N.J: Prentice Hall, 1972. Available at: https://www.google.com.br/books/edition/_/EZImAAAAMAAJ/.
* BRESSAN, G. Linguagens de Implementação de Sistemas e a Linguagem LAPA. 1977. text – Universidade de São Paulo, 1977. DOI 10.11606/D.45.1977.tde-20230303-172219. Available at: https://teses.usp.br/teses/disponiveis/45/45132/tde-20230303-172219/.
* FAPESP. Tomasz Kowaltowski - Biblioteca Virtual da FAPESP. [s. d.]. Available at: https://bv.fapesp.br/pt/pesquisador/92611/tomasz-kowaltowski/.
* GRIES, D. Compiler Construction for Digital Computers. New York: Wiley, 1971. Available at: https://www.google.com.br/books/edition/Compiler_Construction_for_Digital_Comput/CJUEAQAAIAAJ/.
* HOPCROFT, J. E.; ULLMAN, J. D. Formal languages and their relation to automata. USA: Addison-Wesley Longman Publishing Co., Inc., 1969. Available at: https://dl.acm.org/doi/book/10.5555/1096945.
* JENSEN, K.; WIRTH, N. PASCAL User Manual and Report. New York: Springer-Verlag, 1974. Available at: https://archive.org/details/h42_Pascal_User_Manual_and_Report_Second_Edition.
* KNUTH, D. E. The Art of Computer Programming. [S. l.]: Reading, Mass. : Addison-Wesley, 1997. Available at: http://archive.org/details/artofcomputerpro0001knut_l0h13rdedition.
* KOWALTOWSKI, T. Implementação de linguagens de programação. [S. l.]: Guanabara Dois, 1983. Available at: https://scholar.google.com/scholar?cluster=6128347645525656087&hl=en&oi=scholarr.
* LEWIS, P. M.; STEARNS, R. E. Syntax-Directed Transduction. J. ACM, vol. 15, no. 3, p. 465–488, 1 Jul. 1968. DOI 10.1145/321466.321477. Available at: https://dl.acm.org/doi/10.1145/321466.321477.
* MELO, I. S. H. de. Alguns tópicos de compilação e uma implementação da Linguagem Lapa para o Computador Pade. 1978. 1978. Available at: https://bdtd.ibict.br/vufind/Record/USP_25471acc314b810d90b5496267a6b02a.
* NAUR, P. Revised Report on the Algorithmic Language ALGOL 60. Annual Review in Automatic Programming, vol. 4, p. 217–258, 1964. Available at: https://www.sciencedirect.com/science/article/pii/0066413864900205.
* RECHENBERG, P. Sackgassenfreie Syntaxanalyse. Elektronische Rechenanlagen, vol. 15, no. 3,4, p. 119-125,170-176, 1973. Available at: https://www.degruyter.com/document/doi/10.1524/itit.1973.15.16.119/pdf.
* RIPLEY, G. D.; DRUSEIKIS, F. C. Towards a Compiler Error Recovery Effectiveness Rating. Technical Report. Tucson, Arizona: Computer Science Department, The University of Arizona, Apr. 1970. Available at: https://www.amazon.com/Towards-compiler-recovery-effectiveness-rating/dp/B0006XBO2O.
* ROSENKRANTZ, D. J.; STEARNS, R. E. Properties of Deterministic Top-Down Grammars. Information and Control, vol. 17, no. 3, p. 226–256, 1970. Available at: https://core.ac.uk/download/pdf/82034929.pdf.
* SALOMAA, A. Theory of Automata. Oxford: Pergamon Press, 1969. Available at: https://archive.org/details/theoryofautomata0000salo.
* SANCHES, M. M. Portabilidade de Compiladores. 1979. Master’s Thesis – Instituto de Matemática e Estatística da USP, São Paulo, 1979. Available at: https://repositorio.usp.br/item/000711147/.
* SETZER, V. W. Non-recursive Top-down Syntax Analysis. Software: Practice and Experience, vol. 9, no. 3, p. 237–245, 1979. Available at: https://www.academia.edu/113474332/Non_recursive_top_down_syntax_analysis/.
* VELOSO, P. A. S. Máquinas e Linguagens: uma Introdução à Teoria de Autômatos. São Paulo: Escola de Computação, Instituto de Matemática e Estatística da USP, 1979.
* WIJNGAARDEN, A. van; OTHERS. Revised Report on the Algorithmic Language ALGOL 68. Acta Informatica, vol. 5, no. 1–3, p. 1–236, 1975. Available at: https://archive.org/details/revisedreportona0000unse.
* WIRTH, N. The Design of a PASCAL Compiler. Software: Practice and Experience, vol. 1, no. 4, p. 309–333, 1971. DOI 10.1002/spe.4380010403. Available at: https://onlinelibrary.wiley.com/doi/abs/10.1002/spe.4380010403.
* WIRTH, Niklaus. Algorithms + Data Structures = Programs. Englewood Cliffs, New York: Prentice-Hall, 1976. Available at: https://archive.org/details/algorithms-and-data-structures-niklaus-wirth/.
* WIRTH, Niklaus. On “PASCAL”, Code Generation, and the CDC 6000 Computer, n. STAN-CS-72-257. Stanford, California: Computer Science Department, Stanford University, Feb. 1972. Available at: https://archive.org/download/bitsavers_stanfordcs2576600PASCALFeb72_1515774/STAN-CS-72-257_6600_PASCAL_Feb72_text.pdf.


