Limpei o Banco para Normalização dos dados.

Meu foco runtime, budget e revenue, então limpei do banco todos os filmes que não possuiam esses dados.

Continuei minha pesquisa baseada em Tempo de Filme, agora com o Lucro/Projuizo por minuto. 

Formulas DAX:

RevenuePerMin
"Divisão", each [revenue] / [runtime], type number

CostPerMin
"Divisão", each [budget] / [runtime], type number

LucroPrejuizo
"Subtração", each [revenue] - [budget], Int64.Type

ROIMin
"ROIMin", each Value.Divide(([RevenuePerMin] - [CostPerMin]) * [runtime], [budget]) * 100)

Proposta de Modelo Dimensional:
___ Tabela SQL ___

-- Tabela "Movie"
CREATE TABLE Movie (
    id INTEGER PRIMARY KEY,
    title TEXT,
    runtime INTEGER,
    release_date TEXT,
    ano INTEGER,
    vote_average REAL,
    vote_count INTEGER,
    budget INTEGER,
    revenue INTEGER
);

-- Tabela "Genre"
CREATE TABLE Genre (
    id INTEGER PRIMARY KEY,
    name TEXT
);

-- Tabela de relacionamento entre "Movie" e "Genre"
CREATE TABLE MovieGenre (
    movie_id INTEGER,
    genre_id INTEGER,
    PRIMARY KEY (movie_id, genre_id),
    FOREIGN KEY (movie_id) REFERENCES Movie (id),
    FOREIGN KEY (genre_id) REFERENCES Genre (id)
);

-- Tabela "Metrics"
CREATE TABLE Metrics (
    id INTEGER PRIMARY KEY,
    movie_id INTEGER,
    RevenuePerMin REAL,
    CostPerMin REAL,
    LucroPrejuizo INTEGER,
    ROIMin REAL,
    FOREIGN KEY (movie_id) REFERENCES Movie (id)
);

_____
