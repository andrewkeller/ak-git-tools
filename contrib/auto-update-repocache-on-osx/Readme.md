org.git.repocache.update.plist
==============================

This is a `launchd` configuration that invokes `git repocache-update` on a
regular basis (every 6 hours) on the current machine.

Installing
----------

To install, simply place in `~/Library/LaunchAgents` for human users, or
`/Library/LaunchDaemons` for system-level users.  Then, load the configuration
manually using `launchctl(1)`, or by rebooting your computer.

If you have installed the tools in a location that is not included in the
default `PATH`, then you'll need to update the configuration to know about the
new path.  One way to do that is to make the following change to the
`ProgramArguments` array in the configuration:

    diff a/org.git.repocache.update.plist b/org.git.repocache.update.plist
    --- a/org.git.repocache.update.plist
    +++ b/org.git.repocache.update.plist
    @@ -7,6 +7,6 @@
         <key>ProgramArguments</key>
         <array>
             <string>sh</string>
             <string>-c</string>
    -        <string>git repocache-update</string>
    +        <string>PATH=$PATH:/path/to/installation/bin git repocache-update</string>
         </array>

