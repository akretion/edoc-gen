# about edoc-gen

This is a helper script for generating Python and Odoo ERP libraries for the Brazilian electronic fiscal documents using http://www.davekuhlman.org/generateDS.html. It supports all fiscal documents that have XSD schemas, that is: NF-e, CC-e, NFS-e, MDF-e, CT-e, EFD-Reinf, e-Social, GNRE, BP-e...

But why a helper instead of launching generateDS manually?
- For all these fiscal documents there is a common pattern: the schemas are inside a zip archive that can be downloaded from an official URL.
- After normalizing the xsd file names, we launch the generateDS.py generator on them. But we also want to keep our libraries small, so edoc-gen enables to specify that only some xsd files should support both export and import.
- Finally automating all the steps with the proper settings allowed me to quickly re-generate the Python libraries for all the fiscal documents and run the pytests import/export tests on them to ensure any patch in generateDS would would retain real life compatibility (I got some 6 patches merged into generateDS). And mostly it allowed me to the do the same with the Odoo mixin generator plugin: quicky re-generate all Odoo modules and ensure they install and pass the tests. The libraries and Odoo modules are typically distributed as separated packages inside different repos that can get their own bug reports, forks and contributions but it also work to generate everything in the same directory for a quick extensive testing.

# usage

Install generateDS somewhere https://www.davekuhlman.org/generateDS.html#how-to-build-and-install-it

Then export its home directory for the script to know where to find it:
```
export GENERATEDS_HOME=path_to_generateds
```

Optionnally, if you want to generate Odoo models mixins, you need the Akretion plugin for Odoo. It is available in the generateDS pull request for now: https://bitbucket.org/dkuhlman/generateds/pull-requests/51/d7e954682a90/diff .

Either merge the branch, either export an environment variable to the odoo plugin directory with:
```
export ODOO_GEN_HOM=path_to_odoo plugin dir
TODO!!! Too bad it is still hard coded here https://github.com/akretion/edoc-gen/blob/master/scripts/odoo/generate_file
I will fix it soon
```

The script is meant to be run inline from curl so the maintenance can be centralized here without the need to bother packaging the bash scripts. The API is:

```
curl https://raw.githubusercontent.com/akretion/edoc-gen/master/generate | bash -s schema_name plugin_name version \
'schema_url' \
'files'

  were the arguments mean:
- schema_name: name of the schame, can be nfe, nfse, cce, mdfe...
- plugin_name: python|odoo (Akretion made an Odoo plugin for GenarateDS, similar to the Django plugin)
- version: example v4_00
- schema_url: URL of the zip archive containing the schemas
- files: optionally if you want to support XML import of only some XSD files, list them separated by |. ex: 'file1|file2'
```

The script has a modular structure were you can override your own scripts in scripts/gen_plugin to target custom plugin. This projects assumes

* a plugin for a Python library (marshall and unmarshall XML docs according to the provided XSD)
* a plugin for the Odoo open source ERP that generates Odoo mixins to be injected later properly into Odoo models

For each kind of these Brazilian electronic fiscal documents you can find here the generated lib maintained by Akretion and also the basic command that has been used to generate the Python lib. These scripts are only here to demonstrate how it works (you can run them in an empty directory), but the real Python libraries have additional README.rst, tests and limited overrides maintained manually in their respective repo. We also don't generate Python classes (1000 lines minimum) for very small 20 lines schemas such as communication events as these can be easily dealt with simple Python strings or Jinja2.

# NF-e | Nota Fiscal Eletrônica


| Python  | https://github.com/akretion/nfelib           |
|---------|----------------------------------------------|
| Odoo    | https://github.com/OCA/l10n-brazil/pull/658  |


```
curl https://raw.githubusercontent.com/akretion/edoc-gen/master/generate | bash -s nfe python v4_00 \
'http://www.nfe.fazenda.gov.br/portal/exibirArquivo.aspx?conteudo=CoNA9VIgZ3E=' \
'leiauteNFe|leiauteInutNFe'
```

# CC-e | Carta de Correção Eletrônica


| Python  | https://github.com/akretion/ccelib           |
|---------|----------------------------------------------|
| Odoo    | https://github.com/OCA/l10n-brazil/pull/659  |

```
curl https://raw.githubusercontent.com/akretion/edoc-gen/master/generate | bash -s cce python v1_00 \
'http://www.nfe.fazenda.gov.br/portal/exibirArquivo.aspx?conteudo=P/FXaGiLKo0=' \
leiauteCCe
```

# MDF-e | Manifestação do Destinatário Eletrônica


| Python  | https://github.com/akretion/mdfelib          |
|---------|----------------------------------------------|
| Odoo    | https://github.com/OCA/l10n-brazil/pull/660  |

```
curl https://raw.githubusercontent.com/akretion/edoc-gen/master/generate | bash -s mdfe python v3_00 \
'https://dfe-portal.sefazvirtual.rs.gov.br/MDFE/DownloadArquivoEstatico/?sistema=MDFE&tipoArquivo=2&nomeArquivo=PL_MDFe_300_NT022018_v1.02.zip' \
'mdfe|mdfeModalAereo|mdfeModalAquaviario|mdfeModalFerroviario|mdfeModalRodoviario'
```

# CT-e | Conhecimento de Transporte Eletrônico


| Python  | https://github.com/akretion/ctelib           |
|---------|----------------------------------------------|
| Odoo    | https://github.com/OCA/l10n-brazil/pull/661  |

```
curl https://raw.githubusercontent.com/akretion/edoc-gen/master/generate | bash -s cte python v3_00 \
'http://www.cte.fazenda.gov.br/portal/exibirArquivo.aspx?conteudo=xuFsi7DbwBk=' \
'cte|cteModalAereo|cteModalAquaviario|cteModalDutoviario|cteModalFerroviario|cteModalRodoviarioOS|cteModalRodoviario|cteMultiModal'
```

# NFS-e | Nota Fiscal de Serviço padrão nacional ABRASF

| Python  | https://github.com/akretion/nfselib          |
|---------|----------------------------------------------|
| Odoo    | https://github.com/OCA/l10n-brazil/pull/662  |

```
curl https://raw.githubusercontent.com/akretion/edoc-gen/master/generate | bash -s nfse python v2_03 \
'http://www.abrasf.org.br/arquivos/publico/NFS-e/Versao_2.03/schema_xml_nfs-e_%20v2.03.zip' \
nfse
```

# EFD-Reinf | Escrituração Fiscal Digital de Retenções e Outras Informações Fiscais


| Python  | https://github.com/akretion/nfselib          |
|---------|----------------------------------------------|
| Odoo    | https://github.com/OCA/l10n-brazil/pull/666  |

```
curl https://raw.githubusercontent.com/akretion/edoc-gen/master/generate | bash -s efdreinf python v01_04 \
'http://sped.rfb.gov.br/estatico/CA/E40B96DD94D4B54626EDDF0CC3004937AB1597/Pacote%20XSD%20Eventos%20EFD%20Reinf%20v1_04_00.zip'
```

# e-Social | Sistema de Escrituração Digital das Obrigações Fiscais, Previdenciárias e Trabalhistas

| Python  | https://github.com/akretion/esociallib       |
|---------|----------------------------------------------|
| Odoo    | https://github.com/OCA/l10n-brazil/pull/665  |

```
curl https://raw.githubusercontent.com/akretion/edoc-gen/master/generate | bash -s esocial python v02_05 \
'https://portal.esocial.gov.br/manuais/2019-01-29_esquemas_xsd_v02-05-00.zip'
```

# GNRE | Guia Nacional de Recolhimento de Tributos Estaduais

| Python  | https://github.com/akretion/gnrelib          |
|---------|----------------------------------------------|
| Odoo    | https://github.com/OCA/l10n-brazil/pull/663  |

```
curl https://raw.githubusercontent.com/akretion/edoc-gen/master/generate | bash -s gnre python v1_00 \
'http://www.gnre.pe.gov.br/gnre/portal/arquivos/EsquemaLote.zip' \
lote_gnre
```

# BP-e | Bilhete de Passagem Eletrônico

| Python  | https://github.com/akretion/bpelib           |
|---------|----------------------------------------------|
| Odoo    | https://github.com/OCA/l10n-brazil/pull/664  |

```
curl https://raw.githubusercontent.com/akretion/edoc-gen/master/generate | bash -s bpe python v1_00 \
'https://portal.fazenda.sp.gov.br/servicos/bpe/Documents/PL_BPe_100_NT022018.zip' \
'bpe|eventoBPeTiposBasico'
```
