portalpadrao.release
====================

Esse repositório contém os arquivos com as versões utilizadas em cada release do Portal Padrão e uma explicação técnica do funcionamento do lançamento de um novo release.

Passos para criação de um novo release
======================================

A definição do que deve entrar em cada release é definido pela SECOM, com o desenvolvimento das funcionalidades/correções podendo ser feitas com os parceiros ou interessados na comunidade plonegovbr.

Versões e pinagens de novos releases em andamento (ou seja, que não tenham ainda o seu lançamento oficial) são feitas no arquivo https://github.com/plonegovbr/portal.buildout/blob/master/buildout.d/versions.cfg, presente em outro repositório, o https://github.com/plonegovbr/portal.buildout/.

Ações para um novo release
---------------------------

- Se a versão do Plone tiver sido alterada, favor indicar em https://github.com/plonegovbr/brasil.gov.portal#requisitos;
- As dependências plonegovbr (como brasil.gov.tiles, brasil.gov.portlets e etc) tem seus respectivos releases criados, tags geradas e pacotes disponibilizados no pypi;
- Os releases de dependências plonegovbr feitas no passo anterior, as pinagens de dependências externas e do próprio Plone que foram testados durante o processo de geração de release são adicionadas/alteradas em https://github.com/plonegovbr/portal.buildout/blob/master/buildout.d/versions.cfg;
- É feita uma revisão em todas as pinagens de https://github.com/plonegovbr/portal.buildout/blob/master/buildout.d/versions.cfg (procure por FIXME, HACK, BBB) para ver se poderá ser feita alguma modificação para aquele release em específico;
- É feita uma revisão nas issues de https://github.com/plonegovbr/portalpadrao.release/ e https://github.com/plonegovbr/portal.buildout/ para ver se alguma melhoria pode ser incorporada no release;
- Restartar as builds do master das dependências plonegovbr no travis;
- Criar uma instância da última versão de brasil.gov.portal e efetuar um teste exploratório mínimo;
- Quando todas as revisões tiverem sido feitas e estiver para lançar um release, o https://github.com/plonegovbr/portal.buildout/blob/master/buildout.d/versions.cfg será copiado para https://github.com/plonegovbr/portalpadrao.release, criando um novo diretório para aquele release, e o versions-sem-extends.cfg gerado usando o script em https://github.com/plonegovbr/portalpadrao.release/blob/master/versions-sem-extends.sh
- É feita uma alteração no `extends` de https://github.com/plonegovbr/portal.buildout/blob/master/buildout.d/base.cfg informando esse novo release criado (pode ser necessário esperar alguns minutos, pois a url http://downloads.plone.org.br/release/x.x.x/versions.cfg demora um tempo para sincronizar no github);
- Verificar se a build do master de portal.buildout no travis está ok;
- Criação das tags do release (esse é o **último** passo, é nessa ordem caso, durante os passos anteriores, seja necessária alguma alteração pontual):
    - Gerar tag em https://github.com/plonegovbr/portal.buildout;
    - Gerar o versions-sem-extends.cfg usando o script https://github.com/plonegovbr/portalpadrao.release/blob/master/versions-sem-extends.sh passando como parâmetro release gerado no passo anterior, e adicionar na pasta do release em https://github.com/plonegovbr/portalpadrao.release;
    - Gerar tag em https://github.com/plonegovbr/portalpadrao.release;
- Envio de email pra lista plonegovbr indicando o lançamento desse novo release.

Como escolher corretamente o arquivo de versões de um release
-------------------------------------------------------------

A versão escolhida em portalpadrao.release deve ser a mesma que a brasil.gov.portal sendo aceitas variações mínimas como bugfixes de um release (exemplo, o brasil.gov.portal é 1.1.5.1 mas o último release é o 1.1.5.2). 

Pode ser que um release tenha sido lançado e, mesmo testado, ocorra uma situação muito específica que impeça o release de funcionar corretamente (uma dependência errada, uma diretiva de configuração de buildout incorreta, etc). Nesse tipo de situação, é feita a correção e gerado um bugfix do release. Não são feitos bugfixes de releases para adicionar novas funcionalidades pois o escopo já havia sido fechado no momento que ele foi lançado.

Como um release do IDG não envolve apenas o pacote brasil.gov.portal pois existem várias dependências (brasil.gov.\* e vários pacotes da comunidade), não faz sentido mudar a versão de brasil.gov.portal se a modificação que por ventura tenha sido feita para o bugfix daquele release tiver ocorrido em outro pacote: por isso pode ocorrer a situação de um brasi.gov.portal ser a versão 1.1.5.1 mas aqui no portalpadrao.release ser 1.1.5.2.

Por que versions.cfg e versions-sem-extends.cfg? Qual devo usar?
-------------------------------------------------------------

Até o release 1.1.4, o arquivo versions.cfg era um arquivo compilado *manualmente* não contendo nenhuma url a mais no extends.

A partir do release 1.1.5, passamos a usar como referência no extends o versions do Plone para a última versão do Plone homologada pelo IDG, de forma a diminuir o retrabalho na criação desse arquivo de versões. Isso significa que o seu servidor precisará ter acesso externo à essas urls do extends para funcionar corretamente.

Para situações em que você não pode usar o extends de uma url externa (ambientes sem acesso externo), baixe o versions-sem-extends.cfg para a sua instância e renomeie para versions.cfg.

Como devo efetuar a atualização entre releases?
-----------------------------------------------

Ver http://identidade-digital-de-governo-plone.readthedocs.io/en/latest/atualizacao/

Observações finais
------------------

No passado, para releases temporários, se usava o **-pending**, mas isso traz a desvantagem de quebrar todo pacote que referencia o pending quando você tenta voltar o histórico e rodar o buildout e por isso não será mais utilizado.
