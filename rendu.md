## Authors 

Benoît Agogué and Le Goff Thibault

## Question 1

Due to unsafe shellscript programming, Valve's Steam client for Linux had the potential to unintentionally delete all files in every directory on a user's computer. This has been observed to occur among users who have relocated Steam's installation directory.
The bug was due to this piece of code :
```bash
STEAMROOT="$(cd "${0%/*}" && echo $PWD)"

# Scary!
rm -rf "$STEAMROOT/"*
```

The initial step tries to find the script's containing directory. 
However, certain situations could cause it to fail. For instance, if the directory is moved during the script's execution, it would invalidate the "selfpath" variable $0. Similarly, if $0 doesn't contain a slash character or contains a mistyped broken symlink, it would also fail. In the event of a failure, the '&&' conditional ensures that the script produces an empty string without terminating due to the absence of set -e (exiting if previous command fails). 
Despite this failure mode being identified as "Scary!" in the comments, it wasn't checked. In the deletion command, the slash character takes on a  different meaning from its role of path concatenation operator if the string preceding it is empty. It then becomes a means of naming the root directory.

This bug is a local bug, as it localized.

In our opinion, testing the scenario of moving steam library folder would have made it possible to detect the bug.

## Question 2

We chose the  [COLLECTIONS-813] issue.(https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-813?filter=doneissues)

This bug is local as it is localized.

This bug was due to a `NullPointerExeption` that was not thrown.
It was fixed by adding guards to ensure that when values are null, it throws.
The contributors added tests and modified the existing ones in order to detect this bug in the future.

## Question 3

Netflix performs experiment named Chaos Monkey.
This experiment consists in randomly select a virtual machine instances that host the production services and terminates it.

The variables they observe are multiple, like stream start per second or account creation by seconds
They observe the difference between the steady system and the system under attack.

Netflix are note the only one who use this kind of technics. Indeed Facebook and SNCF also work with this system.

Other organization could use this kind of experiment by attacking a system while reducing resources of the cluster.

## Question 4

The advantage of having a formal specification for webassembly is that as the langage is well constructed, it can be safe, fast, portable and compact. So as the architecture is proven, no bugs can came from it.

It does not mean that a formal specification should not be tested as implementation can contains bugs.

## Question 5

The mechanized specification main advantage is that it uncovered
deficiencies that needed to be corrected by the specification authors. In some cases, these meant that the type system was originally unsound.

This mechanism allowed fixing several errors in the WebAssembly specification.

One of the artifact was a proposal to use WebAssembly as a bytecode representation of programs running on the Ethereum virtual machine.

The author verified the specification using type checker and interpreter written with Isabelle/HOL.

This mechanized specification reduce the risk of errors but humans can still make errors so it does not remove the need of testing.