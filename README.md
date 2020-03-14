Neo4j <br/><br/>

Exercise 1 <br/><br/>

Exercise 1.1: Retrieve all nodes from the database.<br/>
match (n) return n
<br/><br/>
Exercise 1.2: Examine the data model for the graph.<br/>
call db.schema.visualization
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
call db.propertyKeys()
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

Exercise 5 <br/><br/>


 Exercise 5.1: Retrieve data using multiple MATCH patterns.<br/>
match (a:Person)-[:ACTED_IN]->(m:Movie)<-[:DIRECTED]-(d:Person), (a2:Person)-[:ACTED_IN]->(m) where a.name = 'Gene Hackman' return m.title as movie, d.name as diretor , a2.name 
<br/><br/>

 Exercise 5.2: Retrieve particular nodes that have a relationship.<br/>
match (p1:Person)-[:FOLLOWS]-(p2:Person) where p1.name = 'James Thompson' return p1, p2
<br/><br/>

 Exercise 5.3: Modify the query to retrieve nodes that are exactly three hops away.<br/>
match (p1:Person)-[:FOLLOWS*3]-(p2:Person) where p1.name = 'James Thompson' return p1, p2
<br/><br/>

 Exercise 5.4: Modify the query to retrieve nodes that are one and two hops away.<br/>
match (p1:Person)-[:FOLLOWS*1..2]-(p2:Person) where p1.name = 'James Thompson' return p1, p2
<br/><br/>
 ssss
 Exercise 5.5: Modify the query to retrieve particular nodes that are connected no matter how many hops are required.<br/>
match (p1:Person)-[:FOLLOWS*]-(p2:Person) where p1.name = 'James Thompson' return p1,p2
<br/><br/>

 Exercise 5.6: Specify optional data to be retrieved during the query.<br/>
match (p:Person) where p.name starts with 'Tom' optional match (p)-[:DIRECTED]->(m:Movie) return p.name, m.title
<br/><br/>
 
 Exercise 5.7: Retrieve nodes by collecting a list. <br/>
match (p:Person)-[:ACTED_IN]->(m:Movie) return p.name,collect(m.title) 
<br/><br/>

Exercise 5.8: Retrieve all movies that Tom Cruise has acted in and the co-actors that acted in the same movie by collecting a list (Solution)<br/>
match (p:Person)-[:ACTED_IN]->(m:Movie)<-[:ACTED_IN]-(p2:Person) where p.name ='Tom Cruise' return m.title, collect(p2.name) 
<br/><br/>

 Exercise 5.9: Retrieve nodes as lists and return data associated with the corresponding lists.<br/>
match (p:Person)-[:REVIEWED]->(m:Movie) return m.title , count(p) , collect(p.name)
<br/><br/>

 Exercise 5.10: Retrieve nodes and their relationships as lists. <br/>
match (d:Person)-[:DIRECTED]->(m:Movie)<-[:ACTED_IN]-(a:Person) return d.name , count(a) , collect(a.name) 
<br/><br/>
 
 Exercise 5.11: Retrieve the actors who have acted in exactly five movies.<br/>
match (a:Person)-[:ACTED_IN]->(m:Movie)
with  a, count(a) AS numMovies, collect(m.title) AS movies where  numMovies = 5 return a.name, movies
<br/><br/>
 
Exercise 5.12: Retrieve the movies that have at least 2 directors with other data.<br/>
match (m:Movie)
with m, size((:Person)-[:DIRECTED]->(m)) AS directors
where directors >= 2
optional match (p:Person)-[:REVIEWED]->(m)
return  m.title, p.name
<br/><br/>

Exercise 6 <br/><br/>

Exercise 6.1: Execute a query that returns duplicate records.<br/>
MATCH (a:Person)-[:ACTED_IN]->(m:Movie)
WHERE m.released >= 1990 AND m.released < 2000
RETURN m.title, collect(a.name)  ssssorder by m.title
<br/><br/>
Exercise 6.2: Modify the query to eliminate duplication.<br/>
MATCH (a:Person)-[:ACTED_IN]->(m:Movie)
WHERE m.released >= 1990 AND m.released < 2000
RETURN  collect(m.title), collect(a.name) order by m.title
<br/><br/>
Exercise 6.3: Modify the query to eliminate more duplication.<br/>
MATCH (a:Person)-[:ACTED_IN]->(m:Movie)
WHERE m.released >= 1990 AND m.released < 2000
RETURN  collect(distinct m.title), collect(distinct a.name) 
<br/><br/>
Exercise 6.4: Sort results returned.<br/>
match (m:Movie) where m.released >= 2000 return m.released, m.title order by m.released, m.title 
<br/><br/>
Exercise 6.5: Retrieve the top 5 ratings and their associated movies.<br/>
match (p:Person)-[r:REVIEWED]->(m:Movie) return m.title, r.rating order by r.rating desc limit 5
<br/><br/>
Exercise 6.6: Retrieve all actors that have not appeared in more than 3 movies.<br/>
match (p:Person)-[:ACTED_IN]->(m:Movie) with p, count(p) as qtd, collect(m.title) as list where qtd <= 3 return p.name, list
<br/><br/>

Exercise 7 <br/><br/>


 Exercise 7.1: Collect and use lists.<br/>
match (p:Person)-[:ACTED_IN]->(m:Movie),(m)<-[:PRODUCED]-(a:Person) return m.title, collect(p.name) as actors, collect(p.name) as producers order by size(actors)
<br/><br/>
 Exercise 7.2: Collect a list.<br/>
match (p:Person)-[:ACTED_IN]->(m:Movie) with p, collect(m.title) as list where size(list) > 5 return p.name, list
<br/><br/>
 Exercise 7.3: Unwind a list.<br/>
match (p:Person)-[:ACTED_IN]->(m:Movie) with p, collect(m.title) as list where size(list) > 5 with p, list unwind list as list1 return p.name, list1
<br/><br/>
 Exercise 7.4: Perform a calculation with the date type.<br/>
match (p:Person)-[:ACTED_IN]->(m:Movie) where p.name = 'Tom Hanks' return m.title, m.released, (date().year - m.released) as yearsAgo, (m.released-p.born) as tomAge order by m.released
<br/><br/> ssss


Exercise 8 <br/><br/>


 Exercise 8.1: Create a Movie node.<br/>
 create (:Movie {title: 'Constantine'})
<br/><br/>
 Exercise 8.2: Retrieve the newly-created node.<br/>
 match (m:Movie) where m.title = 'Constantine' return m.title
<br/><br/>

 Exercise 8.3: Create a Person node.<br/>
create (:Person {name:'Robin Wright'})
<br/><br/>

 Exercise 8.4: Retrieve the newly-created node.<br/>
match (p:Person) where p.name = 'Robin Wright' return p.name
<br/><br/>

 Exercise 8.5: Add a label to a node.<br/>
match (m:Movie) where m.released < 2010 set m:OlderMovie return labels(m)
<br/><br/>

 Exercise 8.6: Retrieve the node using the new label.<br/>
match (o:OlderMovie) return o.released, o.title
<br/><br/>

 Exercise 8.7: Add the Female label to selected nodes.<br/>
 match (p:Person) where p.name starts with 'Robin' set p:Female
<br/><br/>

 Exercise 8.8: Retrieve all Female nodes.<br/>
 match (f:Female) return f.name
<br/><br/>

 Exercise 8.9: Remove the Female label from the nodes that have this label.<br/>
match (f:Female) remove f:Female
<br/><br/>

 Exercise 8.10: View the current schema of the graph.<br/>
CALL db.schema
<br/><br/>

 Exercise 8.11: Add properties to a movie.<br/>
 create (:Movie {title: 'Forrest Gump'})<br/>
 match (m:Movie) where m.title = 'Forrest Gump' set m:OlderMovie, m.released = 1994, m.tagline = "Life is like a box of chocolates…you never know what you’re gonna get.", m.lengthInMinutes = 142

<br/><br/>

 Exercise 8.12: Retrieve an OlderMovie node to confirm the label and properties.<br/>
match (m:Movie) where m.title = 'Forrest Gump' return m.title, m.released, m.lengthInMinutes, m.tagline
<br/><br/>

 Exercise 8.13: Add properties to the person, Robin Wright.<br/>
match (p:Person) where p.name = 'Robin Wright' set p.born = 1966, p.birthPlace = 'Dallas'
<br/><br/>

 Exercise 8.14: Retrieve an updated Person node.<br/>
match (p:Person) where p.name = 'Robin Wright' return p
<br/><br/>

 Exercise 8.15: Remove a property from a Movie node.<br/>
match (m:Movie) where m.title = 'Forrest Gump' remove m.lengthInMinutes
<br/><br/>

 Exercise 8.16: Retrieve the node to confirm that the property has been removed.<br/>
match (m:Movie) where m.title = 'Forrest Gump' return m
<br/><br/>

 Exercise 8.17: Remove a property from a Person node.<br/>
match(p:Person) where p.name = 'Robin Wright' remove p.birthPlace
<br/><br/>

 Exercise 8.18: Retrieve the node to confirm that the property has been removed.<br/>
match(p:Person) where p.name = 'Robin Wright' return p
<br/><br/>


Exercise 9 <br/><br/>

 Exercise 9.1: Create ACTED_IN relationships.<br/>
match (m:Movie) where m.title = 'Forrest Gump' match (p:Person) where (p.name = 'Tom Hanks' or p.name = 'Gary Sinise' or p.name = 'Robin Wright') create (p)-[:ACTED_IN]->(m)
<br/><br/>
 Exercise 9.2: Create DIRECTED relationships.<br/>
 match (m:Movie) where m.title = "Forrest Gump" match (p:Person) where p.name = 'Robert Zemeckis' create (p)-[:DIRECTED]->(m)
<br/><br/>
 
 Exercise 9.3: Create a HELPED relationship.<br/>
match (p:Person) where p.name = 'Tom Hanks' match (p2:Person) where p2.name = "Gary Sinise" create (p)-[:HELPED]->(p2)
<br/><br/>

 Exercise 9.4: Query nodes and new relationships.<br/>
 match (p:Person)-[rel]-(m:Movie) where m.title = "Forrest Gump" return p, rel, m
<br/><br/>

 Exercise 9.5: Add properties to relationships.<br/>
match (p:Person)-[rel]->(m:Movie) where m.title = "Forrest Gump" set rel.roles = case when p.name = "Tom Hanks" then ['Forrest Gump'] when p.name = "Gary Sinise" then ['Lieutenant Dan Taylor'] when p.name = "Robin Wright" then ['Jenny Curran'] end 
<br/><br/>

 Exercise 9.6: Add a property to the HELPED relationship.<br/>
match (p:Person)-[rel:HELPED]->(p2:Person) where p.name = "Tom Hanks" set rel.research = "War History"
<br/><br/>

 Exercise 9.7: View the current list of property keys in the graph.<br/>
call db.propertyKeys()
<br/><br/>

 Exercise 9.8: View the current schema of the graph.<br/>
call db.schema.visualization
<br/><br/>

 Exercise 9.9: Retrieve the names and roles for actors.<br/>
 match (p:Person)-[rel:ACTED_IN]->(m:Movie) where m.title = "Forrest Gump" return p.name as actor, rel.roles
<br/><br/>

 Exercise 9.10: Retrieve information about any specific relationships.<br/>
 match (p:Person)-[rel:HELPED]->(p2:Person) return p.name, p2.name, rel
<br/><br/>

 Exercise 9.11: Modify a property of a relationship.<br/>
 match (p:Person)-[a:ACTED_IN]->(m:Movie) where p.name = "Gary Sinise" and m.title = "Forrest Gump" set a.roles = "Lt. Dan Taylor"
<br/><br/>

 Exercise 9.12: Remove a property from a relationship.<br/>
 match (p:Person)-[h:HELPED]->(p2:Person) where p.name = "Tom Hanks" remove h.research
<br/><br/>

 Exercise 9.13: Confirm that your modifications were made to the graph.<br/>
 match (p:Person)-[a:ACTED_IN]->(m:Movie) where m.title = "Forrest Gump" return p, a, m
<br/><br/>


Exercise 10 <br/><br/>

Exercise 10.1: Delete a relationship.<br/>
 match (p:Person)-[h:HELPED]->(p2:Person) delete h
<br/><br/>
Exercise 10.2: Confirm that the relationship has been deleted.<br/>
  match (p:Person)-[a:ACTED_IN]->(m:Movie) where m.title = "Forrest Gump" return p, a, m
<br/><br/>
Exercise 10.3: Retrieve a movie and all of its relationships.<br/>
 match (p:Person)-[rel]->(m:Movie) where m.title = "The Matrix" return p,m,rel
<br/><br/>
Exercise 10.4: Try deleting a node without detaching its relationships.<br/>
match (m:Movie) where m.title = "The Matrix" delete m<br/>
Cannot delete node<513>, because it still has relationships. To delete this node, you must first delete its relationships.
<br/><br/>
Exercise 10.5: Delete a Movie node, along with its relationships.<br/>
 match (m:Movie) where m.title = "The Matrix" detach delete m
<br/><br/>
Exercise 10.6: Confirm that the Movie node has been deleted.<br/>
 match (m:Movie) where m.title = "The Matrix" return m<br/>
 (no changes, no records)



