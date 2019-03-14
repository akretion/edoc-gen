# edoc-gends

This is a generateDS helper script for generating Python and Odoo ERP libraries for the Brazilian electronic fiscal documents: NF-e, CC-e, NFS-e, MDF-e, CT-e, EFD-Reinf, e-Social, BP-e...  Indeed all these fiscal documents feature a bit the same pattern where the schemas can be downloaded as a zip from an official URL and were we can then apply the generateDS code generator on selected files only. At least only few of these XSD entities need to feature XML importation capabilities while we may keep XML exportations features for all entities.

It is meant to be run with `curl ... | bash -s ...` inline so the maintenance can be centralized here without the need to bother packaging the bash script.

It has a modular structure were you can customize your own scripts in scripts/gen_plugin to target custom plugin. This projects assumes

* a plugin for a Python library (marshall and unmarshall XML docs according to the provided XSD)
* a plugin for the Odoo open source ERP that generates Odoo mixins to be injected later properly into Odoo models

For each kind of these Brazilian electronic fiscal documents you can find here the generated lib maintained by Akretion and also the basic command that has been used to generate the Python lib. These scripts are only here to demonstrate how it works (you can run them in an empty directory), but the real Python libraries have additional README.rst, tests and limited overrides maintained manually in their respective repo.

# NF-e
https://github.com/akretion/nfelib
```
curl https://raw.githubusercontent.com/akretion/edoc-gends/master/generate | bash -s nfe python v4_00 \
'http://www.nfe.fazenda.gov.br/portal/exibirArquivo.aspx?conteudo=CoNA9VIgZ3E=' \
'leiauteNFe|leiauteInutNFe'
```

# CC-e
https://github.com/akretion/ccelib
```
curl https://raw.githubusercontent.com/akretion/edoc-gends/master/generate | bash -s cce python v1_00 \
'http://www.nfe.fazenda.gov.br/portal/exibirArquivo.aspx?conteudo=P/FXaGiLKo0=' \
leiauteCCe
```

# NFS-e (ABRASF)
https://github.com/akretion/nfselib
```
curl https://raw.githubusercontent.com/akretion/edoc-gends/master/generate | bash -s nfse python v2_03 \
'http://www.abrasf.org.br/arquivos/publico/NFS-e/Versao_2.03/schema_xml_nfs-e_%20v2.03.zip' \
nfse
```

# MDF-e,
https://github.com/akretion/ndfelib
```
curl https://raw.githubusercontent.com/akretion/edoc-gends/master/generate | bash -s mdfe python v3_00 \
'https://dfe-portal.sefazvirtual.rs.gov.br/MDFE/DownloadArquivoEstatico/?sistema=MDFE&tipoArquivo=2&nomeArquivo=PL_MDFe_300_NT022018_v1.02.zip' \
mdfe
```

# CT-e
https://github.com/akretion/ctelib
```
curl https://raw.githubusercontent.com/akretion/edoc-gends/master/generate | bash -s cte python v3_00 \
'http://www.cte.fazenda.gov.br/portal/exibirArquivo.aspx?conteudo=xuFsi7DbwBk=' \
cte
```

# EFD-Reinf
https://github.com/akretion/efdlib
```
curl https://raw.githubusercontent.com/akretion/edoc-gends/master/generate | bash -s efdreinf python v01_04 \
'http://sped.rfb.gov.br/estatico/CA/E40B96DD94D4B54626EDDF0CC3004937AB1597/Pacote%20XSD%20Eventos%20EFD%20Reinf%20v1_04_00.zip'
```

# e-Social
https://github.com/akretion/esociallib
```
curl https://raw.githubusercontent.com/akretion/edoc-gends/master/generate | bash -s esocial python v02_05 \
'https://portal.esocial.gov.br/manuais/2019-01-29_esquemas_xsd_v02-05-00.zip'
```

# BP-e
https://github.com/akretion/bpelib
```
TODO
```
