##########
csv-table
##########

.. code-block:: rst

  .. csv-table:: Table 19-1: grep Options
    :header: "Option", "Long Option", "Description"
    :widths: 30, 50, 100

    -i, --ignore-case, "Ignore case. Do not distinguish between uppercase and lowercase characters."
    -v, --invert-match, "Invert match. Normally, grep prints lines that contain a match. This option causes grep to print every line that does not contain a match."




.. csv-table:: Table 19-1: grep Options
  :header: "Option", "Long Option", "Description"
  :widths: 30, 50, 100

  -i, --ignore-case, "Ignore case. Do not distinguish between uppercase and lowercase characters."
  -v, --invert-match, "Invert match. Normally, grep prints lines that contain a match. This option causes grep to print every line that does not contain a match."

* githubのrstでは、widthsの相対比率設定が反映されないようだ。CSSの問題？→TODO
* コンマで列を分けるので、文中にコンマがあってはダメ。エスケープ不可。文全体を" " で囲っておく。
* 上述の目的がなければ" " は省略できる。

テーブルの例はTLCLより。

参考
http://docutils.sourceforge.net/docs/ref/rst/directives.html
