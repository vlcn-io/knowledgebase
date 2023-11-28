Integrate Materialite into Rails and/or this app.

The original pass:
- #2 
- #3
- #4 

Uncovered:

1. A need for `after` and `take` so  we can emit normal JS collections (https://github.com/vlcn-io/materialite/discussions/2)
2. A need for sorted sources so we can
   1. Implement `take` and `after`
   2. Not have to sort the entire result set, allowing for fast query changes in addition to fast incremental updates.


Issues:
- [ ] React virtualized tables are all offset based pagination. We need cursor based for `after`
- [ ] Deletions can retract things from a window. We need the ability to pull more data from the source to re-fill a window
- [ ] Count still requires scanning the entire dataset for initial count
- [ ] 