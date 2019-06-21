# Trapper-Keeper-Annotations

This file is to annotate the functionality and code within a previous group project of mine called Trapper Keeper.


```
app.get('/api/v1/notes', (req, res) => {
  res.status(200).json(app.locals.notes)
});
```
The above block of code allows for fetch requests against a specific url, so the developer can pull all current notes on the server.
- First, we 'GET' against a url, `/api/v1/notes`
- Next, if the status is 200, we use `.json` on that response to make it readable.


```
app.get('/api/v1/notes/:id', (req, res) => {
  const { id } = req.params
  const note = app.locals.notes.find(note => note.id == id);
  if (!note) return res.status(404).json('Note not found');
  return res.status(200).json(note);
});
```
The above block of code is what allows the developer to make changes to a single, specific note.
- First, we make a 'GET' request against `/api/v1/notes/:id`
  - the id of the note we want to edit will be matched to the id in the url
- Next, we destructure id from req.params, which will allow for the matching described above
- Then, we use a simple prototype to find a note with the id from our url request
- After that...
  - If there is no note with that id, we want to return a status of 404 and an error of note not found
  - If there is a note with that id, we return a status of 200 (all good) with a readable, json version of the requested note.
  
  
```
app.post('/api/v1/notes', (req, res) => {
  const { title, tasks } = req.body;
  if (!title || !tasks) return res.status(422).json('Please provide a title and task');
  const newNote = {
    id: Date.now(),
    ...req.body
  };
  app.locals.notes.push(newNote);
  res.status(201).json(newNote);
});
```
The block of code above is what allows a user to make a new note and save to the server.
- First we destructure title and tasks off the request body
- Next, if there no title or task is available, we send back a status of 422 (unprocessible entity) and ask the user to input a title *and* a task.
- Then, if both a title and task are present, we will create a newNote and give the note an id with Date.now
- After that, we push the note into our app.locals and send back a status of 201 (created)


```
app.put('/api/v1/notes/:id', (req, res) => {
  const id = parseInt(req.params.id);
  const { title, tasks } = req.body;
  if(!title || !tasks) return res.status(422).json('Please provide a title and task');
  const noteIndex = app.locals.notes.findIndex(note => note.id === id);
  if (noteIndex === -1) return res.status(404).json('Note not found');
  const updatedCard = { id, title, tasks}
  app.locals.notes.splice(noteIndex, 1, updatedCard);
  return res.sendStatus(204);
});
```
This block of code is what allows a user to update a premade note with a new title or with new tasks.
- First, we set a variable to a parsed version of the id from our request body
- Next, we destructure title and tasks off our request body
- Then, if either title or tasks are missing, we return a status of 422 (unprocessible entity) and ask the user to include both
- Then, we run an array prototype to find the note with an index that matches the id from our first step
- If there is no note with that id, we will return a status of 404 and a message of note not found
- If there is a note with that index, we create a new variable, updatedNote, which contains the updated id, title and tasks from our request, which we destructured in steps one and two
- Next, we take our app.locals, which contains all our current notes, and at the index from step four, we insert our new, updated note.
- Lastly, we return a status of 204 (no content)


```
app.delete('/api/v1/notes/:id', (req, res) => {
  const id = parseInt(req.params.id)
  const noteIndex = app.locals.notes.findIndex(note => note.id === id);
  if(noteIndex === -1) return res.status(404).json('Note does not exist');
  app.locals.notes.splice(noteIndex, 1);
  return res.sendStatus(204);
})
```
The above block of code is what allows a user to delete a card.
- First, we set a variable to a parsed version of the id from our request body
- Next, we use an array prototype to find the index of the note which matches the aforementioned id
- If there is no note with that index, we return a status of 404 and a message of note not found
- If there is a note with the requested index, we remove that note from app.locals with .splice
- Then, we return a status of 204 (no content)
