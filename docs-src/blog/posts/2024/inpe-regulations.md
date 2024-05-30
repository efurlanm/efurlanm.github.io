---
date: 2024-01-24
categories:
  - INPE
---

# INPE General Regulations

INPE's General Regulations 2024 (approved on 09/04/2023) in searchable PDF format:

- [Regimento_Geral_20230904.pdf](../pdf/Regimento_Geral_20230904.pdf) (in Portuguese)

The [original PDF file](<https://www.gov.br/inpe/pt-br/area-conhecimento/posgraduacao/regimentos>) containing INPE's General Regulations 2024 has an annoying problem that is not searchable and is not possible to select text. What I used to fix it was:

```
$ pdfsandwich -lang por <in.pdf> -o <out.pdf>
$ gs -sDEVICE=pdfwrite -dCompatibilityLevel=1.7 -q -o <out.pdf> <in.pdf>
$ ocrmypdf -l=por -s -O=3 --jbig2-lossy --output-type=pdf <in.pdf> <out.pdf>
$ qpdf --linearize <in.pdf> <out.pdf>
```
