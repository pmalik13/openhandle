[Code Examples](OpenHandleCodeExamples.md)
## OpenHandle Lisp Code Examples ##

This page shows an Elisp (Emacs Lisp) example. This makes use of the 'json' library available at [json.el](http://edward.oconnor.cx/elisp/json.el).

Here's how Emacs (using the 'openhandle' library contained in file "[openhandle.el](http://nurture.nature.com/tony/openhandle/code/lisp/openhandle.el)") can grab a handle data record:

```
% emacs -l json.el -l openhandle.el -f openhandle-get-handle
```

prompts for a handle ('10100/nature', say) which gives us:

![http://nurture.nature.com/tony/imx/openhandle_lisp.jpg](http://nurture.nature.com/tony/imx/openhandle_lisp.jpg)

Here's the code for the OpenHandle package:
```
% cat openhandle.el
;;; openhandle.el --- example code for OpenHandle

;; This file is in the Public Domain.

;; Created: 08 April 2008
;; Version: 1.0
;; Keywords: OpenHandle 

;; This file is NOT part of GNU Emacs.

;;; Commentary:

;; This code is an example of querying an OpenHandle server from gnu emacs
;; based on http://code.google.com/p/openhandle/wiki/OpenHandleCodeRuby.

;;; Code:

(require 'url)
(require 'json)

(defvar openhandle-base-url "http://nascent.nature.com/openhandle/handle"
  "Base URL for handle queries.")

(defun openhandle-get-handle (handle)
  "Get data for a HANDLE from `openhandle-base-url'. "
  (interactive "sEnter handle: ")
  (let* ((temp-buffer (openhandle-retrieve-data handle openhandle-base-url))
	 (json (openhandle-parse-json temp-buffer))
	 (result-buffer (get-buffer-create (format "*<hdl:%s>*" handle))))
    (save-excursion
      (kill-buffer temp-buffer)
      (set-buffer result-buffer)
      (erase-buffer)
      (openhandle-format-json json)
      (goto-char (point-min))
      (set-buffer-modified-p nil)
      (display-buffer result-buffer))))

(defun openhandle-retrieve-data (handle &optional base-url)
  "Retrieve the handle data for HANDLE from BASE-URL into a buffer.
BASE-URL defaults to the value of `openhandle-base-url'."
  (unless base-url
    (setq base-url openhandle-base-url))
  (url-retrieve-synchronously (concat base-url "?id=" handle "&format=json")))

(defun openhandle-parse-json (buffer)
  "Parse the json string in BUFFER."
  (save-excursion
    (set-buffer buffer)
    (goto-char (point-min))
    (delete-region (point-min) (search-forward "\n\n"))
    (let ((json-object-type 'hash-table))
      (json-read))))

(defun openhandle-format-json (json)
  "Insert a formatted version of JSON into the current buffer."
  (let* ((values (gethash "handleValues" json))
	 (limit (length values))
	 (index 0))
    (insert "The handle <" (gethash "handle" json) "> has " (format "%d" limit) " values:\n\n")
    (while (< index limit)
      (openhandle-format-value (1+ index) (aref values index))
      (insert "\n")
      (setq index (1+ index)))))

(defvar openhandle-indent-level 0)

(defconst openhandle-indent-step 2)

(defun openhandle-format-value (index value)
  "Insert a formatted version of VALUE into the current buffer."
  (insert "value #" (format "%d" index) ":\n")
  (let ((openhandle-indent-level (1+ openhandle-indent-level)))
    (maphash #'openhandle-format-hashtable value)))

(defun openhandle-format-hashtable (key value)
  (let ((prefix-string (make-string (* openhandle-indent-level openhandle-indent-step) ?\s)))
    (insert prefix-string key " = ")
    (cond
     ((hash-table-p value)
      (insert "{\n")
      (let ((openhandle-indent-level (1+ openhandle-indent-level)))
	(maphash #'openhandle-format-hashtable value))
      (insert prefix-string "}"))
     ((vectorp value)
      (insert "[")
      (openhandle-format-vector value)
      (insert "]"))
     (t
      (insert (if (string= "ttl" key)
		  (format "%d hours" (/ (string-to-number value) 3600))
		value))))
    (insert "\n")))

(defun openhandle-format-vector (vec)
  (unless (null vec)
    (let ((index 0)
	  (limit (length vec)))
      (while (< index limit)
	(when (> index 0)
	  (insert ", "))
	(if (null (aref vec index))
	    (insert "nil")
	  (insert (aref vec index)))
	(setq index (1+ index))))))

(provide 'openhandle)

;;; openhandle.el ends here
```