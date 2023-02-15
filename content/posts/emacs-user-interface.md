+++
title = "Emacs User Interface"
author = ["Shuvrojit Biswas"]
date = 2023-01-17
lastmod = 2023-01-22T05:37:30+06:00
tags = ["ui", "ux", "emacs", "interface"]
categories = ["emacs"]
draft = false
weight = 1003
+++

Aliquam erat volutpat.  Nunc eleifend leo vitae magna.  In id erat non orci commodo lobortis.  Proin neque massa, cursus ut, gravida ut, lobortis eget, lacus.  Sed diam.  Praesent fermentum tempor tellus.  Nullam tempus.  Mauris ac felis vel velit tristique imperdiet.  Donec at pede.  Etiam vel neque nec dui dignissim bibendum.  Vivamus id enim.  Phasellus neque orci, porta a, aliquet quis, semper a, massa.  Phasellus purus.  Pellentesque tristique imperdiet tortor.  Nam euismod tellus id erat.


## Hide ugliness {#hide-ugliness}

Disable all the useless startup ui's. It will start in the scratch buffer.

```emacs-lisp
(setq
 inhibit-startup-message t
 visible-bell t
 initial-scratch-message nil
 ;; Save existing clipboard text into the kill ring before replacing it.
 save-interprogram-paste-before-kill t
 ;; when I say to quit, I mean quit
 confirm-kill-processes nil
 )

(scroll-bar-mode -1)
(tool-bar-mode -1)
(tooltip-mode -1)
(menu-bar-mode -1)
```


## Improve Scrolling {#improve-scrolling}

```emacs-lisp
(setq mouse-wheel-scroll-amount '(1 ((shift) . 1))) ;; one line at a time
(setq mouse-wheel-progressive-speed nil) ;; don't accelerate scrolling
(setq mouse-wheel-follow-mouse 't) ;; scroll window under mouse
(setq scroll-step 1) ;; keyboard scroll one line at a time
```


## Frame transparency {#frame-transparency}

```emacs-lisp
(set-frame-parameter (selected-frame) 'alpha '(99 . 99))
(add-to-list 'default-frame-alist '(alpha . (99 . 99)))
(set-frame-parameter (selected-frame) 'fullscreen 'maximized)
(add-to-list 'default-frame-alist '(fullscreen . maximized))
```


## Line numbers {#line-numbers}

```emacs-lisp
(column-number-mode)

;; Enable line numbers for some modes
(dolist (mode '(text-mode-hook
                prog-mode-hook
                conf-mode-hook))
  (add-hook mode (lambda () (display-line-numbers-mode t))))

;; Override some modes which derive from the above
(dolist (mode '(org-mode-hook))
  (add-hook mode (lambda () (display-line-numbers-mode 0))))
```

```emacs-lisp
;Don't warn for large files (shows up when launching videos)
(setq large-file-warning-threshold nil)

;;Don't warn for following symlinked files
(setq vc-follow-symlinks t)

;Don't warn when advice is added for functions
;;(setq ad-redefinition-action 'accept)
```


## Font {#font}

need to write something about fonts.

```emacs-lisp
;; Set the fixed pitch face
;; sets up org source blocks
(set-face-attribute 'fixed-pitch nil
                    :font "Hack"
                    :weight 'light
                    :height 150
                    )
;;sets up modeline text, code text
(set-face-attribute 'default nil
                    :font "Hack"
                    ;; :font "Cascadia Code"
                    :height 150
                    )

;; Set the variable pitch face
;;sets up org mode header
(set-face-attribute 'variable-pitch nil
                    ;; :font "Cantarell"
                    ;; :font "Inconsolata"
                    ;; :font "ETbookOT"
                    :font "Fira Code"
                    ;; :font "Times New Roman"
                    :weight 'light
                    :height 170)
```


## Modus Theme {#modus-theme}

```emacs-lisp
(use-package modus-themes
  :ensure
  :init
  ;; Add all your customizations prior to loading the themes
  (setq modus-themes-italic-constructs t
        modus-themes-bold-constructs nil
        modus-themes-region '(accented bg-only)
        modus-themes-modeline '(accented borderless padded)
        modus-themes-paren-match '(bold intense)
        modus-themes-markup '(bold underline)
        modus-themes-org-blocks '(tinted-background)
        modus-themes-headings
        '((1 . (background overline variable-pitch 1.5))
          (2 . (overline rainbow 1.3))
          (3 . (overline 1.1))
          (t . (monochrome)))
        )

  ;; Load the theme files before enabling a theme
  (modus-themes-load-themes)
  :config
  (modus-themes-load-vivendi)
  :bind ("<f5>" . modus-themes-toggle))
```


## ef-themes {#ef-themes}

```emacs-lisp
;; (require 'ef-themes)
;; The themes we provide:
;;
;; Light: `ef-day', `ef-light', `ef-spring', `ef-summer'.
;; Dark:  `ef-autumn', `ef-dark', `ef-night', `ef-winter'.

;; We also provide these commands, but do not assign them to any key:
;;
;; - `ef-themes-select'
;; - `ef-themes-load-random'
;; - `ef-themes-preview-colors'
;; - `ef-themes-preview-colors-current'

(use-package ef-themes
  :config
  ;; Disable all other themes to avoid awkward blending:
  (mapc #'disable-theme custom-enabled-themes)

  ;; Load the theme of choice:
  (load-theme 'ef-spring' :no-confirm)
  (load-theme 'ef-winter' :no-confirm)
  (load-theme 'ef-summer' :no-confirm)
  (load-theme 'ef-night' :no-confirm)
  (load-theme 'ef-day' :no-confirm)
  (load-theme 'ef-autumn' :no-confirm)
  )

(ef-themes-load-random 'light)
```


## Beacon {#beacon}

A beacon shinning on your point man.

```emacs-lisp
(use-package beacon
  :init
  (setq beacon-blink-when-point-moves t)
  (setq beacon-blink-when-window-change t)
  (setq beacon-blink-when-window-scrolls t)
  (beacon-mode 1))
```


## Emoji {#emoji}

Emoji support on emacs. :)

```emacs-lisp
(use-package emojify
  :hook (erc-mode . emojify-mode))
(add-hook 'after-init-hook #'global-emojify-mode)
```


## All the icons <span class="tag"><span class="icons">icons</span></span> {#all-the-icons}

```emacs-lisp
(use-package all-the-icons)
```


## Dashboard {#dashboard}

A dashboard that's gonna open everytime you open emacs

```emacs-lisp
(use-package dashboard
  :ensure t
  :config
  (dashboard-setup-startup-hook))
```


## Solarized Theme+Modeline {#solarized-theme-plus-modeline}

```emacs-lisp
;;; module-solarized.el --- solarized module for my emacs

;; Author: Mark Feller <mark.feller@member.fsf.org>

;; This file is not part of GNU Emacs.

;; This file is free software; you can redistribute it and/or modify
;; it under the terms of the GNU General Public License as published by
;; the Free Software Foundation; either version 3, or (at your option)
;; any later version.

;; This file is distributed in the hope that it will be useful,
;; but WITHOUT ANY WARRANTY; without even the implied warranty of
;; MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
;; GNU General Public License for more details.

;; You should have received a copy of the GNU General Public License
;; along with this file.  If not, see <http://www.gnu.org/licenses/>.

;;; Commentary:

;;; Code:

(use-package solarized-theme
  :config
  (progn (setq solarized-emphasize-indicators nil
               solarized-high-contrast-mode-line nil
               solarized-scale-org-headlines nil
               solarized-use-less-bold t
               solarized-use-variable-pitch nil
               solarized-distinct-fringe-background nil)))

(use-package all-the-icons
  :demand
  :init
  (progn (defun -custom-modeline-github-vc ()
           (let ((branch (mapconcat 'concat (cdr (split-string vc-mode "[:-]")) "-")))
             (concat
              (propertize (format " %s" (all-the-icons-octicon "git-branch"))
                          'face `(:height 1 :family ,(all-the-icons-octicon-family))
                          'display '(raise 0))
              (propertize (format " %s" branch))
              (propertize "  "))))

         (defun -custom-modeline-svn-vc ()
           (let ((revision (cadr (split-string vc-mode "-"))))
             (concat
              (propertize (format " %s" (all-the-icons-faicon "cloud"))
                          'face `(:height 1)
                          'display '(raise 0))
              (propertize (format " %s" revision) 'face `(:height 0.9)))))

         (defvar mode-line-my-vc
           '(:propertize
             (:eval (when vc-mode
                      (cond
                       ((string-match "Git[:-]" vc-mode) (-custom-modeline-github-vc))
                       ((string-match "SVN-" vc-mode) (-custom-modeline-svn-vc))
                       (t (format "%s" vc-mode)))))
             face mode-line-directory)
           "Formats the current directory."))
  :config
  (progn
    ;; (setq-default mode-line-format
    ;;               (list
    ;;                evil-mode-line-tag
    ;;                mode-line-front-space
    ;;                mode-line-mule-info
    ;;                mode-line-modified
    ;;                mode-line-frame-identification
    ;;                mode-line-buffer-identification
    ;;                " "
    ;;                mode-line-position
    ;;                mode-line-my-vc
    ;;                mode-line-modes))
    ;; (concat evil-mode-line-tag)
    ))


;; (bind-keys ("C-c tl" . (lambda () (interactive) (load-theme 'solarized-light)))
;;            ("C-c td" . (lambda () (interactive) (load-theme 'solarized-dark))))

(load-theme 'solarized-light t)

(set-face-attribute 'mode-line nil
                    :background "#eee8d5"
                    :foreground "#657b83"
                    :box '(:line-width 4 :color "#eee8d5")
                    :overline nil
                    :underline nil)

(set-face-attribute 'mode-line-inactive nil
                    :background "#fdf6e3"
                    :foreground "#93a1a1"
                    :box '(:line-width 4 :color "#eee8d5")
                    :overline nil
                    :underline nil)

(define-minor-mode minor-mode-blackout-mode
  "Hides minor modes from the mode line."
  t)

(catch 'done
  (mapc (lambda (x)
          (when (and (consp x)
                     (equal (cadr x) '("" minor-mode-alist)))
            (let ((original (copy-sequence x)))
              (setcar x 'minor-mode-blackout-mode)
              (setcdr x (list "" original)))
            (throw 'done t)))
        mode-line-modes))

(global-set-key (kbd "C-c m") 'minor-mode-blackout-mode)
;; ;; window dividers
;; (window-divider-mode t)
;; (setq window-divider-default-right-width 2)

;; (set-face-attribute 'window-divider nil :foreground "#eee8d5")
;; (set-face-attribute 'window-divider-first-pixel nil :foreground "#eee8d5")
;; (set-face-attribute 'window-divider-last-pixel nil :foreground "#eee8d5")

(provide 'module-solarized)

;;; module-solarized.el ends here
```


## Ewal {#ewal}

Based on pywal. You can find more about  pywal in arch.

```emacs-lisp
(use-package ewal
  :init (setq ewal-use-built-in-always-p nil
              ewal-use-built-in-on-failure-p t
              ewal-built-in-palette "sexy-material"))
(use-package ewal-spacemacs-themes
  :init (progn
          (setq spacemacs-theme-underline-parens t
                my:rice:font (font-spec
                              :family "Source Code Pro"
                              :weight 'semi-bold
                              :size 11.0))
          (show-paren-mode +1)
          (global-hl-line-mode)
          (set-frame-font my:rice:font nil t)
          (add-to-list  'default-frame-alist
                        `(font . ,(font-xlfd-name my:rice:font))))
  :config (progn
            (load-theme 'ewal-spacemacs-modern t)
            (enable-theme 'ewal-spacemacs-modern)))
(use-package ewal-evil-cursors
  :after (ewal-spacemacs-themes)
  :config (ewal-evil-cursors-get-colors
           :apply t :spaceline t))
(use-package spaceline
  :after (ewal-evil-cursors winum)
  :init (setq powerline-default-separator nil)
  :config (spaceline-spacemacs-theme))
```


## Emacs Mini frame {#emacs-mini-frame}

```emacs-lisp
(use-package mini-frame)
(setq x-gtk-resize-child-frames 'resize-mode)
(custom-set-variables
 '(mini-frame-show-parameters
   '((top . 400)
     (width . 0.7)
     (left . 0.5))))
```
