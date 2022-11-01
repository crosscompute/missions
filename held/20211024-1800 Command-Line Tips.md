# Log

## Sunday 20211024-1815 - 20211024-1830: 15 minutes

Today's vim tip is to change window split orientation.

    # Vertical => Horizontal
    CTRL-w t CTRL-w K
    # Horizontal => Vertical
    CTRL-w t CTRL-w H

Actually, I don't think we even need to type CTRL-w t. It seems to work with just CTRL-w K and CTRL-w H.

    # Select top left window
    CTRL-w t
    # Switch to left right split
    CTRL-w H
    # Switch to top bottom split
    CTRL-w K

Today's tmux tip is to split your command-line window. Note that you can set a custom keyboard shortcut for calling tmux.

    vim ~/.tmux.conf
        bind-key C-b send-prefix

    CTRL-b "  # Split horizontally
    CTRL-b %  # Split vertically
