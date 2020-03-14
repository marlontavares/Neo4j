Neo4j <br/><br/>

Exercise 1 <br/><br/>

Exercise 1.1: Retrieve all nodes from the database.<br/>
match (n) return n
<br/><br/>
Exercise 1.2: Examine the data model for the graph.<br/>
call db.schema()
<br/><br/>
Exercise 1.3: Retrieve all Person nodes.<br/>
match (p:Person) return p
<br/><br/>
Exercise 1.4: Retrieve all Movie nodes.<br/>
match (m:Movie) return m
<br/><br/>

Exercise 2 <br/><br/>

Exercise 2.1: Retrieve all movies that were released in a specific year.<br/>
match (m:Movie {released:2000}) return m
<br/><br/>
Exercise 2.2: View the retrieved results as a table.<br/>
Resultado em tabela <br/>
{
  "title": "Jerry Maguire",
  "tagline": "The rest of his life begins now.",
  "released": 2000
}<br/>
{
  "title": "The Replacements",
  "tagline": "Pain heals, Chicks dig scars... Glory lasts forever",
  "released": 2000
}<br/>
{
  "title": "Cast Away",
  "tagline": "At the edge of the world, his journey begins.",
  "released": 2000
}

<br/><br/>
Exercise 2.3: Query the database for all property keys.<br/>
call db.propertyKeys
<br/><br/>
Exercise 2.4: Retrieve all Movies released in a specific year, returning their titles. <br/>
match (m:Movie {released: 2003}) return m.title
<br/><br/>
Exercise 2.5: Display title, released, and tagline values for every Movie node in the graph. <br/>
match (m:Movie) return m.title, m.released, m.tagline
<br/><br/>
Exercise 2.6: Display more user-friendly headers in the table <br/>
match (m:Movie) return m.title AS `movie title`, m.released AS released, m.tagline AS tagLine
<br/><br/>

Exercise 3 <br/><br/>

Exercise 3.1: Display the schema of the database.<br/>
call db.schema
<br/><br/>
Exercise 3.2: Retrieve all people who wrote the movie Speed Racer.<br/>
MATCH (p:Person)-[:WROTE]->(:Movie {title: 'Speed Racer'}) RETURN p.name
<br/><br/>
Exercise 3.3: Retrieve all movies that are connected to the person, Tom Hanks.<br/>
match (m:Movie)<--(:person {name:'Tom Hanks'}) return m.title
<br/><br/>
Exercise 3.4: Retrieve information about the relationships Tom Hanks had with the set of movies retrieved earlier.<br/>
match (m:movie)-[rel]-(:person {name: 'Tom Hanks'}) return m.title, type(rel)
<br/><br/>
Exercise 3.5: Retrieve information about the roles that Tom Hanks acted in.<br/>
match (m:Movie)-[rel:ACTED_IN]-(:Person {name:'Tom Hanks'}) return m.title, rel.roles
<br/><br/>

Exercise 4 <br/><br/>


Exercise 4.1: Retrieve all movies that Tom Cruise acted in.<br/>
match (a:Person)-[:ACTED_IN]->(m:Movie)
where a.name = 'Tom Cruise'
return m.title as Movie
<br/><br/>
Exercise 4.2: Retrieve all people that were born in the 70’s.
<br/>
MATCH (a:Person)
WHERE a.born >= 1970 AND a.born < 1980
RETURN a.name as Name, a.born as `Year Born`
<br/><br/>
 Exercise 4.3: Retrieve the actors who acted in the movie The Matrix who were born after 1960.
<br/>
MATCH (a:Person)-[:ACTED_IN]->(m:Movie)
WHERE a.born > 1960 AND m.title = 'The Matrix'
RETURN a.name as Name, a.born
<br/><br/>
 Exercise 4.4: Retrieve all movies by testing the node label and a property.
<br/>
MATCH (m)
WHERE m:Movie AND m.released = 2000
RETURN m.title
<br/><br/>
 Exercise 4.5: Retrieve all people that wrote movies by testing the relationship between two nodes.
<br/>
MATCH (a)-[rel]->(m)
WHERE a:Person AND type(rel) = 'WROTE' AND m:Movie
RETURN a.name as Name, m.title 
<br/><br/>
 Exercise 4.6: Retrieve all people in the graph that do not have a property.
<br/>
MATCH (a:Person) WHERE NOT exists(a.born) RETURN a.name as Name
<br/><br/>
Exercise 4.7: Retrieve all people related to movies where the relationship has a property.
<br/>
match (a:Person)-[rel]->(m:Movie) where exists(rel.rating) return a.name, m.title, rel.rating 
<br/><br/>
 Exercise 4.8: Retrieve all actors whose name begins with James.
<br/>
match (a:Person)-[:ACTED_IN]->(:Movie) where a.name starts with 'James' return a.name
<br/><br/>
 Exercise 4.9: Retrieve all all REVIEW relationships from the graph with filtered results.
<br/>
match (:Person)-[r:REVIEWED]->(m:Movie) WHERE toLower(r.summary) contains 'fun' return  m.title 
<br/><br/>
 Exercise 4.10: Retrieve all people who have produced a movie, but have not directed a movie.
<br/>
match (a:Person)-[:PRODUCED]->(m:Movie) where not (a)-[:DIRECTED]->(:Movie) return a.name, m.title
<br/><br/>
 Exercise 4.11: Retrieve the movies and their actors where one of the actors also directed the movie.
<br/>
match (a1:Person)-[:ACTED_IN]->(m:Movie)<-[:ACTED_IN]-(a2:Person) where exists( (a2)-[:DIRECTED]->(m) ) return  a1.name as ator1, a2.name as ator2, m.title
<br/><br/>
 Exercise 4.12: Retrieve all movies that were released in a set of years.
<br/>
match (m:Movie) where m.released in [200,2004,2008] return m.title, m.released
<br/><br/>
 Exercise 4.13: Retrieve the movies that have an actor’s role that is the name of the movie.
<br/>
match (a:Person)-[r:ACTED_IN]->(m:Movie) where m.title in r.roles return a.name, m.title
<br/><br/>







