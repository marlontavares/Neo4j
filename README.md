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










