<div dir=rtl>بنام خدا</div>

- [ByoBu](#byobu)



### ByoBu

- Confige:
  - byobu-enable & byobu-disable set as default and unset
  - byobu-select-backend (1:tmux 2:screen)
  - byobu-enable-prompt : enable color full, after command should run `~/.bashrc`
##### Using Sessions  
  - <kbd>CTRL</kbd><kbd>+</kbd><kbd>SHIFT</kbd><kbd>+</kbd><kbd>F2</kbd> will create a new session.
  - <kbd>ALT</kbd><kbd>+</kbd><kbd>UP</kbd> and <kbd>ALT</kbd><kbd>+</kbd><kbd>DOWN</kbd> will scroll through your sessions.
  - <kbd>F6</kbd> will detach your current Byobu session.
  - <kbd>SHIFT</kbd><kbd>+</kbd><kbd>F6</kbd> will detach (but not close) Byobu, and will maintain your SSH connection to the server. You can get back to Byobu with the byobu command.
  - <kbd>ALT</kbd><kbd>+</kbd><kbd>F6</kbd> will detach all connections to Byobu except for the current one.
#####Using Windows
  - <kbd>F2</kbd> creates new windows within the current session.
  - <kbd>F3</kbd> and <kbd>F4</kbd> scroll left and right through the windows list.
  - <kbd>CTRL</kbd><kbd>+</kbd><kbd>SHIFT</kbd><kbd>+</kbd><kbd>F3</kbd>/<kbd>F4</kbd> moves a window left and right through the windows list.
##### Using Panes
  - <kbd>F8</kbd> renames the current open window in the list.
  - <kbd>F7</kbd> lets you view scrollback history in the current window.
  - <kbd>SHIFT</kbd><kbd>+</kbd><kbd>F2</kbd> creates a horizontal pane; <kbd>CTRL</kbd><kbd>+</kbd><kbd>F2</kbd> creates a vertical one.
  - <kbd>SHIFT</kbd><kbd>+</kbd><kbd>LEFT</kbd>/<kbd>RIGHT</kbd>/<kbd>UP</kbd>/<kbd>DOWN</kbd> or <kbd>SHIFT</kbd><kbd>+</kbd><kbd>F3</kbd>/<kbd>F4</kbd> switches between panes.
  - <kbd>CTRL</kbd><kbd>+</kbd><kbd>F3</kbd>/<kbd>F4</kbd> moves the current pane up or down, respectively.
  - <kbd>SHIFT</kbd><kbd>+</kbd><kbd>ALT</kbd><kbd>+</kbd><kbd>LEFT</kbd>/<kbd>RIGHT</kbd>/<kbd>UP</kbd>/<kbd>DOWN</kbd> resizes the current pane.
  - <kbd>SHIFT</kbd><kbd>+</kbd><kbd>F11</kbd> toggles a pane to fill the whole window temporarily.
  - <kbd>ALT</kbd><kbd>+</kbd><kbd>F11</kbd> splits a pane into its own new window permanently.
##### Using Status Notifications
  - arch shows the system architecture, i.e. x86_64.
      `battery` shows the current battery level (for laptops).
  - date shows the current system date.
  - disk shows the current disk space usage.
  - hostname shows the current system hostname.
  - ip_address shows the current system IP address.
  - load_average shows the current system load average.
  - memory shows the current memory usage.
  - network shows the current network usage, sending and receiving.
  - reboot_required shows an indicator when a system reboot is required.
  - release shows the current distribution version (e.g. 14.04).
  - time shows the current system time.
  - updates_available shows an indicator when there are updates available.
  - uptime shows the current system uptime.
  - whoami shows the currently logged in user.
aftter *Apply* you may need <kbd>F5</kbd> to refresh


[Top](#top)


