---
layout: post
title:  "Howto add new qmp command"
date:   2016-05-02 10:00:00
categories: Virtualization
tags: Virtualization
---
### Background

Tell you how to add new command for qmp protocol based on Qemu Upstream. Just for learning.

### SourceCodes

- Commit Message, you can find out the steps for adding command and usage.

```sh
commit f2850a22df4dc605f3b963f87ed6d469d51041ee
Author: Wei Jiangang <weijg.fnst@cn.fujitsu.com>
Date:   Thu May 26 14:29:25 2016 +0800

qmp: add command 'my-command'

1) define new structure 'MyType' (new file example.json)
2) include the new file (example.json)
3) define new command 'my-command' in qapi-schema.json.
4) define  in qmp-commands.hx
   It's unuseful that the code between SQMP and EQMP.
5) implement the function 'qmp_my_command'.

Example:

-> { "execute": "my-command", "arguments": { "id": 88 } }
<- {
     "return": {
         "id": 88
     }
   }

-> { "execute": "my-command", "arguments": { "id": 88, "desc": "This my command" } }
<- {
     "return": {
         "desc": "This my command",
         "id": 88
     }
   }

```

- Codes 

```sh
diff --git a/qapi-schema.json b/qapi-schema.json
index 01fb7b2..de8f291 100644
--- a/qapi-schema.json
+++ b/qapi-schema.json
@@ -20,6 +20,9 @@
 # QAPI introspection
 { 'include': 'qapi/introspect.json' }
 
+# QAPI example definitions
+{ 'include': 'qapi/example.json'}
+
 ##
 # @LostTickPolicy:
 #
@@ -4181,3 +4184,16 @@
 # Since: 2.6
 ##
 { 'command': 'query-gic-capabilities', 'returns': ['GICCapability'] }
+
+##
+# @my-command:
+#
+# This command is used for example
+#
+# Returns: a MyType object
+#
+# Since: 2.6
+##
+{ 'command': 'my-command',
+  'data': { 'id': 'int', '*desc': 'str' },
+  'returns': 'MyType' }
diff --git a/qapi/example.json b/qapi/example.json
new file mode 100644
index 0000000..7728347
--- /dev/null
+++ b/qapi/example.json
@@ -0,0 +1,4 @@
+# -*- Mode: Python -*-
+#
+# QAPI example definitions
+{ 'struct': 'MyType', 'data': { 'id': 'int', '*desc': 'str' } }
diff --git a/qmp-commands.hx b/qmp-commands.hx
index 2bf3d58..498f556 100644
--- a/qmp-commands.hx
+++ b/qmp-commands.hx
+@@ -4901,3 +4901,27 @@ Example:
                  { "version": 3, "emulated": false, "kernel": true } ] }
  
 EQMP
+
+    {
+        .name = "my-command",
+        .args_type  = "id:i,desc:s?",
+        .mhandler.cmd_new = qmp_marshal_my_command,
+    },
+
+
+SQMP
+my-command
+---------------
+
+Return a MyType object
+
+Arguments:
+
+- "id": the ID (json-int)
+- "desc": additional argument (json-string, optional)
+
+Example:
+
+-> { "execute": "my-command", "arguments": { "id": "88" } }
+<- { "return": { "id": 88 } }
+EQMP
diff --git a/qmp.c b/qmp.c
index 0690347..f94043b 100644
--- a/qmp.c
+++ b/qmp.c
@@ -70,6 +70,23 @@ VersionInfo *qmp_query_version(Error **errp)
     return info;
 }
 
+// qmp_marshal_my_command() : be generated automatically.
+// qmp_my_command() : be defined by user, and called by qmp_marshal_my_command
+MyType *qmp_my_command(int64_t id, bool has_desc, const char *desc, Error **errp)
+{
+    MyType *mytype = g_new0(MyType, 1);
+
+    mytype->id = id;
+
+    if (has_desc) {
+        // If miss setting has_desc, It won't output the member 'desc'
+        mytype->has_desc = true;
+        mytype->desc = g_strdup(desc);
+    }
+
+    return mytype;
+}
+
 KvmInfo *qmp_query_kvm(Error **errp)
 {
     KvmInfo *info = g_malloc0(sizeof(*info));
```

**There's nothing impossible to him who will try !**
