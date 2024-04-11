---
title: "First_post"
date: 2024-03-27T13:11:06+01:00
tags : ['angr','python','picoCTF2024']
categories : ['ctf-writeups']
author: "Katana"
# author: ["Me", "You"] # multiple authors
showToc: true
TocOpen: false
draft: false
hidemeta: false
comments: false
description: "testing only "
canonicalURL: "https://canonical.url/to/page"
disableHLJS: true # to disable highlightjs
disableShare: false
disableHLJS: false
hideSummary: false
searchHidden: true
ShowReadingTime: true
ShowBreadCrumbs: true
#ShowPostNavLinks: true
ShowWordCount: true
ShowRssButtonInSectionTermList: true
UseHugoToc: true
summary : 'PicoCTF crackme101 with ANGR'
---
# ANGR

this is a challnge from the last picoctf 2024 its named
_crackme101_

```python
import angr
import claripy
import sys

proj = angr.Project("./crackme100", auto_load_libs=False) 

flag = claripy.BVS('flag', 8*50)  

state = proj.factory.full_init_state(
        
        add_options=angr.options.unicorn,
        stdin=angr.SimPackets(name='stdin', content=[(flag, 50)]),
        #remove_options={angr.options.LAZY_SOLVES}

)
for i in range(50):
    state.solver.add(flag.get_byte(i) >=b'a')
    state.solver.add(flag.get_byte(i) <=b'z')
    

def is_successful(state):
    stdout_output = state.posix.dumps(sys.stdout.fileno())
    return b"SUCCESS" in stdout_output

def should_abort(state):
    stdout_output = state.posix.dumps(sys.stdout.fileno())
    return b"FAILED!" in stdout_output

sm = proj.factory.simulation_manager(state)
sm.explore(find=is_successful, avoid=should_abort)
sm.run()
if sm.found:
    sol = sm.found[0]
    print(sol.posix.dumps(sys.stdin.fileno()))
else:
    print("no sol")
```
