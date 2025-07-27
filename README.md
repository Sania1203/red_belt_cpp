This is my final project for C++ red belt.
Part 1 - SearchServer:
AddQueriesStream(istream& query_input, ostream& search_results_output):
Commits search. 
One request = one line in query_input.
Every search request consists of words, divided by one or numerous spaces.
Words consist of lowercase latin letters.
Search result is maximum 5 most relevant documents.

UpdateDocumentBase(istream& document_input):
Changes current document base to the document_input.

The task was optimisation of a slow program version.
Limits: 
document_input <= 50 000 documents;
every document <= 1 000 words;
all different words number in all documents <= 10 000;
max word length = 100 symbols;
query_input <= 500 000 requests, every one is 1 .. 10 words;

Part 2 - simulation of a multi-thread web-server:
Testing functions used: 
void TestSearchServer(vector<pair<istream, ostream*>> streams) {
  //IteratorRange — template of a Paginator task
  //random_time() — function, returning a random time prtiod

  LOG_DURATION("Total");
  SearchServer srv(streams.front().first);
  for (auto& [input, output] :
       IteratorRange(begin(streams) + 1, end(streams))) {
    this_thread::sleep_for(random_time());
    if (!output) {
      srv.UpdateDocumentBase(input);
    } else {
      srv.AddQueriesStream(input, *output);
    }
  }
}

Methods AddQueriesStream and UpdateDocumentBase are parallel threading ready.