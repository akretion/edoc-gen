# edoc-gen

This is a helper script for generating Python and Odoo ERP libraries for the Brazilian electronic fiscal documents using http://www.davekuhlman.org/generateDS.html. It supports all fiscal documents that have XSD schemas, that is: NF-e, CC-e, NFS-e, MDF-e, CT-e, EFD-Reinf, e-Social, GNRE, BP-e...

For all these fiscal documents the schemas are inside a zip arcchive that can be downloaded from an official URL. After normalizing the xsd file names, we launch the generateDS.py generator on them. But we want to keep our libraries small, so gen-ds enables to specify that only some xsd files should support both export and import.

The script is meant to be run inline from curl so the maintenance can be centralized here without the need to bother packaging the bash scripts. The API is:

```
curl https://raw.githubusercontent.com/akretion/edoc-gen/master/generate | bash -s schema_name plugin_name version \
'schema_url' \
'files'

  were the arguments mean:
- schema_name: name of tthe schame, can be nfe, nfse, cce, mdfe...
- plugin_name: python|odoo (Akretion made an Odoo plugin for GenarateDS, similar to the Django plugin)
- version: example v4_00
- schema_url: URL of the zip archive containing the schemas
- files: optionally if you want to support XML import of only some XSD files, list them separated by |. ex: 'file1|file2'
```

The script has a modular structure were you can override your own scripts in scripts/gen_plugin to target custom plugin. This projects assumes

* a plugin for a Python library (marshall and unmarshall XML docs according to the provided XSD)
* a plugin for the Odoo open source ERP that generates Odoo mixins to be injected later properly into Odoo models

For each kind of these Brazilian electronic fiscal documents you can find here the generated lib maintained by Akretion and also the basic command that has been used to generate the Python lib. These scripts are only here to demonstrate how it works (you can run them in an empty directory), but the real Python libraries have additional README.rst, tests and limited overrides maintained manually in their respective repo.

# NF-e
https://github.com/akretion/nfelib
```
curl https://raw.githubusercontent.com/akretion/edoc-gen/master/generate | bash -s nfe python v4_00 \
'http://www.nfe.fazenda.gov.br/portal/exibirArquivo.aspx?conteudo=CoNA9VIgZ3E=' \
'leiauteNFe|leiauteInutNFe'
```

# CC-e
https://github.com/akretion/ccelib
```
curl https://raw.githubusercontent.com/akretion/edoc-gen/master/generate | bash -s cce python v1_00 \
'http://www.nfe.fazenda.gov.br/portal/exibirArquivo.aspx?conteudo=P/FXaGiLKo0=' \
leiauteCCe
```

# NFS-e (ABRASF)
https://github.com/akretion/nfselib
```
curl https://raw.githubusercontent.com/akretion/edoc-gen/master/generate | bash -s nfse python v2_03 \
'http://www.abrasf.org.br/arquivos/publico/NFS-e/Versao_2.03/schema_xml_nfs-e_%20v2.03.zip' \
nfse
```

# MDF-e,
https://github.com/akretion/ndfelib
```
curl https://raw.githubusercontent.com/akretion/edoc-gen/master/generate | bash -s mdfe python v3_00 \
'https://dfe-portal.sefazvirtual.rs.gov.br/MDFE/DownloadArquivoEstatico/?sistema=MDFE&tipoArquivo=2&nomeArquivo=PL_MDFe_300_NT022018_v1.02.zip' \
'mdfe|mdfeModalAereo|mdfeModalAquaviario|mdfeModalFerroviario|mdfeModalRodoviario'
```

# CT-e
https://github.com/akretion/ctelib
```
curl https://raw.githubusercontent.com/akretion/edoc-gen/master/generate | bash -s cte python v3_00 \
'http://www.cte.fazenda.gov.br/portal/exibirArquivo.aspx?conteudo=xuFsi7DbwBk=' \
'cte|cteModalAereo|cteModalAquaviario|cteModalDutoviario|cteModalFerroviario|cteModalRodoviarioOS|cteModalRodoviario|cteMultiModal'
```

# EFD-Reinf
https://github.com/akretion/efdlib
```
curl https://raw.githubusercontent.com/akretion/edoc-gen/master/generate | bash -s efdreinf python v01_04 \
'http://sped.rfb.gov.br/estatico/CA/E40B96DD94D4B54626EDDF0CC3004937AB1597/Pacote%20XSD%20Eventos%20EFD%20Reinf%20v1_04_00.zip'
```

# e-Social
https://github.com/akretion/esociallib
```
curl https://raw.githubusercontent.com/akretion/edoc-gen/master/generate | bash -s esocial python v02_05 \
'https://portal.esocial.gov.br/manuais/2019-01-29_esquemas_xsd_v02-05-00.zip'
```

# GNRE
https://github.com/akretion/gnrelib
```
curl https://raw.githubusercontent.com/akretion/edoc-gen/master/generate | bash -s gnre python v1_00 \
'http://www.gnre.pe.gov.br/gnre/portal/arquivos/EsquemaLote.zip'
```

# BP-e
https://github.com/akretion/bpelib
```
curl https://raw.githubusercontent.com/akretion/edoc-gen/master/generate | bash -s bpe python v1_00 \
'https://portal.fazenda.sp.gov.br/servicos/bpe/Documents/PL_BPe_100_NT022018.zip'
```
