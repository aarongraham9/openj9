<!--
Copyright (c) 2018, 2021 IBM Corp. and others

This program and the accompanying materials are made available under
the terms of the Eclipse Public License 2.0 which accompanies this
distribution and is available at https://www.eclipse.org/legal/epl-2.0/
or the Apache License, Version 2.0 which accompanies this distribution and
is available at https://www.apache.org/licenses/LICENSE-2.0.

This Source Code may also be made available under the following
Secondary Licenses when the conditions for such availability set
forth in the Eclipse Public License, v. 2.0 are satisfied: GNU
General Public License, version 2 with the GNU Classpath
Exception [1] and GNU General Public License, version 2 with the
OpenJDK Assembly Exception [2].

[1] https://www.gnu.org/software/classpath/license.html
[2] http://openjdk.java.net/legal/assembly-exception.html

SPDX-License-Identifier: EPL-2.0 OR Apache-2.0 OR GPL-2.0 WITH Classpath-exception-2.0 OR LicenseRef-GPL-2.0 WITH Assembly-exception
-->

# JITServer Front-end

When a server is compiling methods, it needs to know lots of information about the client VM. JITServer uses front-end, class/VM environment, and object model classes to encapsulate most of the remote calls.

In a non-JITServer JVM, the front-end (instantiated as `TR_J9VM`) is a class that contains queries for JIT to communicate with the rest of the VM.

The front-end, typically instantiated as `TR_J9VM`, is specialized for JITServer on the server as `TR_J9ServerVM` in the file `env/VMJ9Server.hpp`. This is done in a manner similarly to how `TR_J9SharedCacheVM` is used for AOT, except the overridden functionality is quite different. For JITServer, most overridden methods send messages to query information from the `TR_J9VM` on the client. In some cases cached data is accessed instead of performing a remote call. By doing this, we can make the server compile code which is compatible with the client side VM.
