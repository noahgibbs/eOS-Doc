# Kernellib

Dgd has [some unusual characteristics](../dgd/unusual.html). It also provides enormous flexibility. Kernellib provides control over those unusual characteristics, and provides sensible defaults on top of that flexibility.

## Persistence

One of the major functions of Kernellib is to manage persistence. DGD potentially allows fully recompiling all objects, but in practice that has some limitations. For somewhat-complicated reasons, you can't safely recompile a ***parent*** object that has other objects inherit from it while persisting its state (such as any data fields in the object.)

The Kernellib handles this by requiring all ***parent classes*** to be ***abstract parent classes***. If it's possible to inherit from an object then you can't instantiate it.

It does this through a combination of naming and object management.

## Naming

DGD normally names an object by where it occurs in the directory hierarchy. An object in /usr/System/initd.c is normally referred to as "/usr/System/initd" by DGD when you deal with it. It's common to define a constant (that is, an ALL_CAPS_NAME) for each object, but the value of that constant is the path to the object's code.

In Kernellib, that name determines some important things about the object's capabilities. For instance, if the name contains /usr/SomeUser, then SomeUser is considered the creator of that object, which determines what files the object can read and write. Different capabilities can be set in Kernellib for different directories, allowing objects from those directories to do (or not do) particular things.

Naming is also used to determine whether a particular object is a parent object (if its path contains /lib/ anywhere), a normal instantiable object (normally uses /obj/ or /sys/ rather than lib) or a lightweight object that doesn't manage its own memory (/data/).

Thus, in Kernellib the naming determines both the ***type*** of the object (parent, child, LWO) and the ***permissions*** for the object (creator, directory.)

## Object Management

DGD has the ability for your application to register an Object Manager. In a language where you can fully recompile any object, it's useful to tell when this has happened and update appropriately. The Object Manager receives these events from DGD and may respond to them.

The Kernellib doesn't supply an object manager, but it does provide a few more events for an Object Manager to receive than raw DGD does.

## Inheritance and Object Life Cycle

Kernellib makes sure that if an object is inherited ***from***, it's a parent object whose path contains /lib/ somewhere. It makes sure if it's ***inheriting*** then it's ***not*** a parent object.

That way, parent objects will only exist as part of child objects and will never have their own state that needs to be carefully saved. They are state-free and can be recompiled (and if necessary, destroyed) freely without disrupting any other objects.

With an object manager, you can also set callbacks on objects to make them respond appropriately to being compiled, instantiated, updated or destroyed.
