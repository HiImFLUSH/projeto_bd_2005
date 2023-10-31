# projeto_bd_2005
Projeto de Bases de Dados de League of Legends em SQL


Neste projeto tive como base os dados fornecidos pelo jogo League of Legends, mais especificamente da Lore criada para os personagens e de dados de análises de partidas como Itens, Roles e Funções in-game. Dessa forma, o objetivo será a criação de uma base de dados utilizando tais conhecimentos de forma que haja uma complexidade de relações e informações traçadas pelos critérios predefinidos.

O jogo possui uma gama de personagens (Champions) que possuem diversas características intrínsecas a cada um e que definem a forma como se deve jogar com eles. São atributos tanto como status de campeão (HP, Mana, Energia, Dano Físico e Dano Mágico, etc.) - que são escalados de acordo com o nível durante a partida - como as habilidades únicas que produzem diferentes efeitos e determinam o “Kit” de cada campeão. Essas duas características também configuram qual a função e a role específica de todos os champions.

Para cada personagem é atribuída também uma história única que pode se correlacionar com a de outros campeões (Lore), ela vem de forma a explicar as origens e construir uma personalidade para cada um. Portanto, foi criado um universo fictício do jogo onde serão exploradas informações como Deuses, Locais de nascença, Guerras entre países, Economia, Religião, Militarização, Relações entre amigos e inimigos, Familiaridade/Criação, etc.

1.	Modelo ER
![image](https://github.com/HiImFLUSH/projeto_bd_2005/assets/92924024/e7cd0de1-c7c9-4b94-bdfb-e0d03c614221)

 
Após a confecção estrutural dos conceitos do modelo, chegamos ao diagrama em modelo ER mostrado acima com as seguintes entidades:


CHAMPION - Possui atributos baseados na Lore do personagem e data de lançamento do campeão no jogo.
●	Name: atributo chave de cada personagem. Cada nome é exclusivo, portanto Champions diferentes não podem ter o mesmo nome;
●	Sex?: atributo opcional, visto que nem todos os personagens são humanos. Existem algumas raças e entidades divinas no jogo que não possuem sexo definido, como por exemplo Aurelion Sol, um Dragão Estelar e Blitzcrank, um robô;
●	ReleaseDate: atributo de valor simples que se refere a data de lançamento inicial do personagem no jogo;
●	[SeasonRelease]: atributo derivado de ReleaseDate, resultando na temporada de lançamento do Champion;
●	{Relationships}: define as afinidades de cada personagem, assim como as interações entre cada campeão dentro do universo do jogo. É um atributo multivalor, uma vez que um campeão pode conhecer vários outros e ter diferentes relações.

REGION - Possui atributos respectivos a cada região do universo do jogo.
●	RegionId: identifica cada região com a abreviação do seu nome. Atributo chave para identificar a região;
●	Name: atributo de valor simples para o nome das regiões;
●	Politics(Religious?, Military?, Economic?): um atributo composto em que é expressa a geopolítica de cada região, com os valores para definir a forma de cada governo e as características da região e população.

CLASSGROUP - Possui atributos respectivos a cada Champion. Caracteriza a função e a role baseada no kit de habilidades e status de cada campeão.
●	ClassGroup: atributo chave para identificar o nome de cada grupo de classes;
●	{Role({Function})}: atributo multi-valor composto por outro atributo multi-valor, que define as rotas que cada campeão comumente é atribuído. Cada rota pode desempenhar uma ou mais funções em uma partida. Um campeão pode ser designado a diferentes rotas por partida.

 
CLASS - Possui atributos respectivos a cada Champion, de forma análoga à entidade CLASSGROUP. Define a classe onde cada um se encaixa (Tipo de dano e Items comuns à classe a que pertence).
●	ClassName: atributo chave para definir uma classe específica;
●	MainDamage: atributo simples para classificar o tipo de dano que um campeão causa;
●	{Main Items}: atributo composto para indicar quais itens um campeão costuma equipar. Um personagem pode se equipar com mais de um item a depender do seu tipo de dano ou ClassGroup.


➤ Sobre os relacionamentos:

KNOWS - Relacionamento entre Champions, com grau de relacionamento vários-para-vários e participação parcial para ambos os lados. Determinado Champion pode, ou não, ter alguma relação com um ou vários Champions diferentes.

COMMANDEDBY - Relacionamento um-para-um de participação parcial para regiões e campeões. Uma região pode ou não ser governada por um Champion e um Champion pode ou não governar uma região.

HASCLASS - Relacionamento vários-para-vários de participação total entre Champions e Class. Um personagem deve ter uma classe ou pode ter várias, enquanto uma classe deve ter pelo menos um personagem ou pode ter vários.

BELONGSIN - Relacionamento um-para-vários de participação total entre ClassGroup e Class. Uma ou mais classes fazem parte de apenas uma ClassGroup, enquanto uma ClassGroup tem uma ou mais classes associadas.

HASBORN - Relacionamento vários-para-um entre Champions e Region, sendo total para os campeões e parcial para região, visto que um ou mais personagens nasceram numa região, mas não necessariamente uma região teve um personagem nascido.

 

2.	Modelo Relacional
 ![image](https://github.com/HiImFLUSH/projeto_bd_2005/assets/92924024/fcb980e1-cffc-4a43-bf8e-332bdce9fa12)


Baseando-se no diagrama de modelo Entidade-Relação criado, foi transformado para modelo Relacional conforme o diagrama de tabelas mostrado acima.

Cada tabela se configura nos moldes estabelecidos pelo diagrama ER e se caracterizam da seguinte forma:

CHAMPIONS: Possui o atributo RegisterNumber como chave primária, que identifica um personagem por um número de registo específico, atribuído a cada personagem. Esta tabela apresenta as características de cada Champion e possui o atributo LocalOfBirth como chave externa para a tabela Region, com a finalidade de definir a relação HASBORN entre o campeão e a região a qual pertence.

 
RELATIONSHIPS: Com os atributos InitialChampion , FinalChampion e Relation como chaves primárias, esta tabela representa o relacionamento entre dois personagens, podendo essas relações serem bilaterais ou unilaterais. Possui como chaves externas os atributos InitialChampion e FinalChampion para a tabela CHAMPION, onde esses dois atributos fazem referência ao número de registo dos personagens. Esta foi a forma encontrada para relacionar por tabela a ligação KNOWS entre dois personagens durante a transição do diagrama ER para Relacional.

REGION: Com o atributo RegionID definido como chave primária, possui também uma chave externa através do atributo Governor, que pode assumir valor NULL. A chave externa faz referência à chave primária da tabela Champion, RegisterNumber, e representa a relação parcial um-para-um, COMMANDEDBY, entre um campeão governante e uma região. A tabela também simboliza as características de cada país, ou território, e o relacionamento entre um Champion e as diferentes regiões do jogo.

CHAMPIONCLASSES: Com os atributos ChampionID e Class, ambos como chaves primárias, esses atributos são também chaves externas para diferentes tabelas, notadamente, ChampionID para a tabela Champion e Class para a tabela Class. Esta é uma tabela auxiliar que representa a relação total vários-para-vários, HASCLASS, entre um Champion e as diferentes classes.

CLASSGROUP: Com os atributos ClassGroup e Role como chaves primárias, esta tabela define a forma como cada atribuição aos campeões pode ser dada através da tradução da relação BELONGSIN, com funções que variam de acordo com a rota e a classe de um campeão específico.

CLASS: O atributo ClassName é definido como chave primária para esta tabela, que tem definida como chave externa o atributo ClassGroup que faz referência à tabela ClassGroup. Estas duas tabelas representam a relação em que uma ou várias classes fazem parte de uma ClassGroup.

ITEMS: Possui como chave primária os atributos ClassName e Item, sendo ClassName chave externa para a Tabela Class. Esta é uma tabela auxiliar criada a partir da relação entre a entidade-tipo Class e o atributo multi-valor Main Items.
	
 

3.	 Volume de Dados

Para o desenvolvimento desta base de dados, foram então criadas sete tabelas e o número de registos em cada uma é descrito abaixo. Para este efeito foi utilizado o comando SELECT COUNT(*), no terminal do MySQL, para cada tabela.

	CHAMPIONS: 154 registos.

RELATIONSHIPS: 183 registos.

REGION: 14 registos.

CHAMPIONCLASSES: 202 registos.

CLASSGROUP: 16 registos.

CLASS: 17 registos.

ITEMS: 73 registos.
