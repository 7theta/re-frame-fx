# com.7theta/re-frame-fx

[![Current Version](https://img.shields.io/clojars/v/com.7theta/re-frame-fx.svg)](https://clojars.org/com.7theta/re-frame-fx)
[![GitHub license](https://img.shields.io/github/license/7theta/re-frame-fx.svg)](LICENSE)
[![Circle CI](https://circleci.com/gh/7theta/re-frame-fx.svg?style=shield)](https://circleci.com/gh/7theta/re-frame-fx)
[![Dependencies Status](https://jarkeeper.com/7theta/re-frame-fx/status.svg)](https://jarkeeper.com/7theta/re-frame-fx)

A set of [re-frame](https://github.com/Day8/re-frame)
[Effect Handlers](https://github.com/Day8/re-frame/tree/develop/docs) in
common use across 7theta projects.

## Registering the effects handler

The namespace where event handlers are registered, typically
`events.cljs`, is the generally the place where the effects handler
can be registered with re-frame.

In order to register the effects handlers, simply `require` the relevant
handler namespaces.

## Using the effects handler

#### :dispatch-debounce

[Debounces](https://css-tricks.com/the-difference-between-throttling-and-debouncing/#article-header-id-1)
the dispatching of events. Events will only dispatch after `timeout` ms
if no subsequent event has been received with the same `:id`. If such
and event is received, the timer will start again.

This approach is useful for dealing with spikes in events where the
processing of intermediate events is not useful until things have
"settled", e.g., scrolling, resizing windows etc.

The handler can accept either a map or a sequence of maps. The
example below uses a sequence, however the sequence is not necessary
if there is only a single dispatch.

```cljs
{:dispatch-debounce [{:id ::calcaulate-positions-after-resize
                      :timeout 250
                      :action :dispatch
                      :event [:recompute-positons]}]}
```

`:dispatch-n` is also a valid action and accepts a sequence
of events in the `:event` key.

Deferred (debounced) events can be cancelled by issuing a `:cancel`
action.

```cljs
{:dispatch-debounce {:id ::calcaulate-positions-after-resize
                     :action :cancel}}
```

Alternatively events can be forced to dispatch ignoring the timer by
using a `:flush` action.

```cljs
{:dispatch-debounce {:id ::calcaulate-positions-after-resize
                     :action :flush}}
```

## Copyright and License

Copyright © 2015, 2016, 2017 7theta

Distributed under the Eclipse Public License.
