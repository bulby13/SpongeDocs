===================
Working with Events
===================

Sponge provides a system to (1) fire events and then (2) listen for events.

Overview
========

Events are used to inform plugins of certain happenings. Many events can also be *cancelled* -- that is, the action that the event refers to can be prevented from occurring. Cancellable events implement the ``Cancellable`` interface.

Sponge itself contains many events; however, plugins can create their own events which other plugins can subscribe to.

Subscribing is the act of listening to an event. An event handler is what subscribes to an event. Event handlers are assigned a priority that determines the order in which the event handler is run in context of other event handlers (such as those from other plugins). For example, an event handler with *EARLY* priority will return before most other event handlers.

Events cannot be sent to a specific set of plugins. All plugins that subscribe to an event will be notified of the event.

The event bus or event manager is the class that keeps track of which plugins have subscribed to which event, and is also responsible for distributing events to event handlers. Sponge comes with an event bus.

An important note about events in Sponge is that the event bus **supports supertypes**. For example, there's a ``BlockBreakEvent`` that extends a ``BlockChangeEvent``. A plugin could subscribe to the ``BlockChangeEvent`` and still receive ``BlockBreakEvent`` events. However, a plugin subscribed to just ``BlockBreakEvent`` would not receive other types of ``BlockChangeEvent``.

The event bus in Sponge is a high-performance event bus.

Event Handlers
==============

In order to listen for event, an event handler must be registered. This is done by making a method with any name, having the first (and only) parameter be of the desired event type that you want to catch, and then affixing ``@Subscribe`` to the method. This is illustrated below.

.. code-block:: java

    import org.spongepowered.api.util.event.Subscribe;

    @Subscribe
    public void someEventHandler(SomeEvent event) {
        // Do something with the event
    }
    
In addition, the object that has these methods must be registered with the event bus.

.. code-block:: java

    eventBus.register(theObject);

The event bus is the ``org.spongepowered.api.service.event.EventManager`` class. See :doc:`services` to learn how to get access to this instance.

.. tip::

    For event handlers on your main plugin class, you do not need to register the object for events because Sponge will do it automatically.
    
Note that, by default, ``@Subscribe`` is configured so that your event handler will *not* be called if the event in question is cancellable and has been cancelled (such as by another plugin).

.. note::
    
    It is currently not possible to register a dynamic event handler by providing an event type and a handler.

About @Subscribe
~~~~~~~~~~~~~~~~

The ``@Subscribe`` annotation has a few configurable fields:

* ``order`` is the order in which the event handler is to be run. See the ``Order`` enum in Sponge to see the available options.
* ``ignoreCancelled``, if true (which is default true), causes the event handler to be skipped if the event in question is cancellable and has been cancelled (by a previously-executed plugin, for example).

Firing Events
=============

Firing Sponge Events
~~~~~~~~~~~~~~~~~~~~

Creating Custom Events
======================

Callbacks
=========

Callbacks are a more advanced feature of Sponge's event system.
