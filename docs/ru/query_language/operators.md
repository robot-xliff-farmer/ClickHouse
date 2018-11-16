# Operators

All operators are transformed to the corresponding functions at the query parsing stage, in accordance with their precedence and associativity. Groups of operators are listed in order of priority (the higher it is in the list, the earlier the operator is connected to its arguments).

## Access Operators

`a[N]` Access to an element of an array; `arrayElement(a, N) function`.

`a.N` – Access to a tuble element; `tupleElement(a, N)` function.

## Numeric Negation Operator

`-a` – The `negate (a)` function.

## Multiplication and Division Operators

`a * b` – The `multiply (a, b) function.`

`a / b` – The `divide(a, b) function.`

`a % b` – The `modulo(a, b) function.`

## Addition and Subtraction Operators

`a + b` – The `plus(a, b) function.`

`a - b` – The `minus(a, b) function.`

## Comparison Operators

`a = b` – The `equals(a, b) function.`

`a == b` – The `equals(a, b) function.`

`a != b` – The `notEquals(a, b) function.`

`a <> b` – The `notEquals(a, b) function.`

`a <= b` – The `lessOrEquals(a, b) function.`

`a >= b` – The `greaterOrEquals(a, b) function.`

`a < b` – The `less(a, b) function.`

`a > b` – The `greater(a, b) function.`

`a LIKE s` – The `like(a, b) function.`

`a NOT LIKE s` – The `notLike(a, b) function.`

`a BETWEEN b AND c` – The same as `a >= b AND a <= c.`

## Operators for Working With Data Sets

*See the section "IN operators".*

`a IN ...` – The `in(a, b) function`

`a NOT IN ...` – The `notIn(a, b) function.`

`a GLOBAL IN ...` – The `globalIn(a, b) function.`

`a GLOBAL NOT IN ...` – The `globalNotIn(a, b) function.`

## Logical Negation Operator

`NOT a` The `not(a) function.`

## Logical AND Operator

`a AND b` – The`and(a, b) function.`

## Logical OR Operator

`a OR b` – The `or(a, b) function.`

## Conditional Operator

`a ? b : c` – The `if(a, b, c) function.`

Note:

The conditional operator calculates the values of b and c, then checks whether condition a is met, and then returns the corresponding value. If `b` or `C` is an [arrayJoin()](functions/array_join.md#functions_arrayjoin) function, each row will be replicated regardless of the "a" condition.

<a name="operator_case"><a></p> 

<h2>
  Conditional Expression
</h2>

<pre><code class="sql">CASE [x]
    WHEN a THEN b
    [WHEN ... THEN ...]
    ELSE c
END
</code></pre>

<p>
  If "x" is specified, then transform(x, \[a, ...\], \[b, ...\], c). Otherwise – multiIf(a, b, ..., c).
</p>

<h2>
  Concatenation Operator
</h2>

<p>
  <code>s1 || s2</code> – The <code>concat(s1, s2) function.</code>
</p>

<h2>
  Lambda Creation Operator
</h2>

<p>
  <code>x -&gt; expr</code> – The <code>lambda(x, expr) function.</code>
</p>

<p>
  The following operators do not have a priority, since they are brackets:
</p>

<h2>
  Array Creation Operator
</h2>

<p>
  <code>[x1, ...]</code> – The <code>array(x1, ...) function.</code>
</p>

<h2>
  Tuple Creation Operator
</h2>

<p>
  <code>(x1, x2, ...)</code> – The <code>tuple(x2, x2, ...) function.</code>
</p>

<h2>
  Associativity
</h2>

<p>
  All binary operators have left associativity. For example, <code>1 + 2 + 3</code> is transformed to <code>plus(plus(1, 2), 3)</code>. Sometimes this doesn't work the way you expect. For example, <code>SELECT 4 &gt; 2 &gt; 3</code> will result in 0.
</p>

<p>
  For efficiency, the <code>and</code> and <code>or</code> functions accept any number of arguments. The corresponding chains of <code>AND</code> and <code>OR</code> operators are transformed to a single call of these functions.
</p>

<h2>
  Checking for <code>NULL</code>
</h2>

<p>
  ClickHouse supports the <code>IS NULL</code> and <code>IS NOT NULL</code> operators.
</p>

<p>
  

<a name="operator-is-null"></a>

</p>

<h3>
  IS NULL
</h3>

<ul>
  <li>
    For <a href="../data_types/nullable.md#data_type-nullable">Nullable</a> type values, the <code>IS NULL</code> operator returns: <ul>
      <li>
        <code>1</code>, if the value is <code>NULL</code>.
      </li>
      <li>
        <code>0</code> otherwise.
      </li>
    </ul>
  </li>
  <li>
    For other values, the <code>IS NULL</code> operator always returns <code>0</code>.
  </li>
</ul>

<pre><code class="bash">:) SELECT x+100 FROM t_null WHERE y IS NULL

SELECT x + 100
FROM t_null
WHERE isNull(y)

┌─plus(x, 100)─┐
│          101 │
└──────────────┘

1 rows in set. Elapsed: 0.002 sec.
</code></pre>

<p>
  

<a name="operator-is-not-null"></a>

</p>

<h3>
  IS NOT NULL
</h3>

<ul>
  <li>
    For <a href="../data_types/nullable.md#data_type-nullable">Nullable</a> type values, the <code>IS NOT NULL</code> operator returns: <ul>
      <li>
        <code>0</code>, if the value is <code>NULL</code>.
      </li>
      <li>
        <code>1</code> otherwise.
      </li>
    </ul>
  </li>
  <li>
    For other values, the <code>IS NOT NULL</code> operator always returns <code>1</code>.
  </li>
</ul>

<pre><code class="bash">:) SELECT * FROM t_null WHERE y IS NOT NULL

SELECT *
FROM t_null
WHERE isNotNull(y)

┌─x─┬─y─┐
│ 2 │ 3 │
└───┴───┘

1 rows in set. Elapsed: 0.002 sec.
</code></pre>