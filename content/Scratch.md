- prior: #2 

`take(n)` is an operator to constrain the returned set to some window.

Implementing `take(n)` in a streaming setting has some interesting considerations which depend on whether or not the source is sorted.
- #4 

# take(n) & sorted source

1. The lower bound of `take(n)` is the _least value_ (and thus _first value_) seen. First value given the input source is sorted.
   1. `take(n)` can be chained after `after(cursor)` to pin the lower bound to a known value. E.g., for pagination.
3. The upper bound of `take(n)` is the _max value_ we've seen upon reaching `n`. This may not be the last value given more values can be emitted by the upstream in response to changes in the source. See #4 for why this is true.
4. `take(n)` will stop taking from the source (during a full recomputation) once its window is filled. It will continue subscribing to updates, however, so it can maintain the values of the records in the window.

When seeing values outside the window, `take(n)` drops them and does not forward them to downstream operators. This ensures the downstreams only ever operate on the window.

When a value is _removed_ from the window it is unclear what to do. The window now has a hole. We could wait for an event to fill this hole but this may never happen. Added and modified things are likely to be out of range. Because of (3), `take(n)` does not know about things outside the window.

What `take(n)` needs to do is to be able to communicate back up to the source and ask for more data _after_ the max of its window.

# take(n) & unsorted source

1. There is no concept of "bounds" when taking from an unsorted source
2. Once the window is filled it is filled
3. The window only processes changes to records already within the window

(3) means we need some way to model `update` rather than just `add` and `remove`. That or once something is removed from the window we take whatever the next thing that is added and put it into the window. This likely makes the most sense.

This variant of take still has the issue with holes opening up in the window albeit slightly less of an issue since any future addition will fill the hole.

It is also unclear how an unsorted take should ask for more data from the source. Does it just get any random value not part of the current window?

Another interpretation is that an unsorted take is a chronological take. The windows keeps moving forward as events accumulate, dropping old things and adding new things.