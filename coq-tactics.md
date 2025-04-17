---
permalink: /coq-tactics/
---
<div class="main coqindex">
        <p id="dnlogo">
            <img src="/static/img/coq.png">
        </p>
        <h1>Coq Tactics Index</h1>
        <h3>Stage 1: Proving Easy Goals</h3>
<p><a href="#reflexivity"><code>reflexivity</code></a><br>
<a href="#assumption"><code>assumption</code></a><br>
<a href="#discriminate"><code>discriminate</code></a><br>
<a href="#constructor"><code>constructor</code></a>  </p>
<h3>Stage 2: Transforming Your Goal</h3>
<p><a href="#apply"><code>apply</code></a><br>
<a href="#subst"><code>subst</code></a><br>
<a href="#rewrite"><code>rewrite</code></a><br>
<a href="#simpl"><code>simpl</code></a><br>
<a href="#cut"><code>cut</code></a><br>
<a href="#unfold"><code>unfold</code></a></p>
<h3>Stage 3: Breaking Apart Your Goal</h3>
<p><a href="#destruct"><code>destruct</code></a><br>
<a href="#inversion"><code>inversion</code></a><br>
<a href="#induction"><code>induction</code></a>  </p>
<h3>Stage 4: Powerful Automatic Tactics</h3>
<p><a href="#auto"><code>auto</code></a><br>
<a href="#intuition"><code>intuition</code></a><br>
<a href="#omega"><code>omega</code></a>  </p>
<hr>
<h2><a name="reflexivity"></a><code>reflexivity</code></h2>
<p>Use <code>reflexivity</code> when your goal is to prove that something equals itself.</p>
<p>In this example we will prove that any term <code>x</code> of type <code><span class="coq_lblue">Set</span></code> is equal to itself. After we intro the variable we can prove the goal using <code>reflexivity</code>.</p>
<div class="example">
<div class="code">
<pre><span class="checked"><span class="coq_red">Lemma</span> <span class="coq_blue">everything_is_itself:</span>
  <span class="coq_gren">forall</span> x: <span class="coq_lblue">Set</span>, x = x.
<span class="coq_purple">Proof.</span>
  intro.</span>
  reflexivity.
<span class="coq_purple">Qed.</span>
</pre>
</div>
<div class="context">
<pre>1 subgoal
x : <span class="coq_lblue">Set</span>
-----------(1/1)
x = x
</pre>
</div>
</div>

<p><strong>Use it when:</strong> your goal is something like <code>a = a</code>.</p>
<p><strong>Advanced usage:</strong> <code>reflexivity</code> will work even if your goal is not syntactically identical on the left and right side of the equality. Both sides just have to <em>evaluate</em> to the same term.</p>
<p>In this example we will apply <code>reflexivity</code> to a more complicated math equation: (3 + (0 + 2)) = (1 + 4).</p>
<div class="example">
<div class="code">
<pre><span class="checked"><span class="coq_red">Inductive</span> nat : <span class="coq_lblue">Set</span> :=
  | O
  | S : nat -&gt; nat.

<span class="coq_red">Fixpoint</span> <span class="coq_blue">add</span> (a: nat) (b: nat) : nat :=
  <span class="coq_gren">match</span> a <span class="coq_gren">with</span>
    | O =&gt; b
    | S x =&gt; S (add x b)
  <span class="coq_gren">end</span>.

<span class="coq_red">Lemma</span> <span class="coq_blue">complex_math:</span>
    (add
        (S (S (S O)))
        (add O (S (S O)))) =
    add (S O) (S (S (S (S O)))).
<span class="coq_purple">Proof.</span>
    reflexivity.</span>
<span class="coq_purple">Qed.</span>
</pre>
</div>
<div class="context">
<pre>No more subgoals.
</pre>
</div>
</div>

<h2><a name="assumption"></a><code>assumption</code></h2>
<p>If the thing you are trying to prove is already in your context, use <code>assumption</code> to finish the proof.</p>
<p>In this example we show that if we assume <code>p</code> we can prove <code>p</code>. We use <code>assumption</code> to tell Coq that our goal is already true in our context because we assumed it!</p>
<div class="example">
<div class="code">
<pre><span class="checked"><span class="coq_red">Lemma</span> <span class="coq_blue">everything_implies_itself:</span>
  <span class="coq_gren">forall</span> p: <span class="coq_lblue">Prop</span>, p -&gt; p.
<span class="coq_purple">Proof.</span>
  intros.</span>
  assumption.
<span class="coq_purple">Qed.</span>
</pre>
</div>
<div class="context">
<pre>1 subgoal
p : <span class="coq_lblue">Prop</span>
H : p
-----------(1/1)
p
</pre>
</div>
</div>

<p><strong>Use it when:</strong> your goal is already in your "context" of terms you already know.</p>
<h2><a name="discriminate"></a><code>discriminate</code></h2>
<p>If you have an equality in your context that isn't true, you can prove anything using <code>discriminate</code>.</p>
<p>For <code>discriminate</code> to work, the terms must be "structurally" different. This means that both terms are elements of an inductive set but they are built differently, using different constructors (e.g. <code>true</code> and <code>false</code>, or <code>(S O)</code> and <code>(S (S O))</code>).</p>
<p>In this example we show that if we assume <code>true = false</code> then we can prove anything. Note that we don't specify what <code>a</code> is, it really can be anything!</p>
<div class="example">
<div class="code">
<pre><span class="checked"><span class="coq_red">Inductive</span> <span class="coq_blue">bool:</span> <span class="coq_lblue">Set</span> :=
  | true
  | false.

<span class="coq_red">Lemma</span> <span class="coq_blue">incorrect_equality_implies_anything:</span>
  <span class="coq_gren">forall</span> a, false = true -&gt; a.
<span class="coq_purple">Proof.</span>
  intros.</span>
  discriminate.
<span class="coq_purple">Qed.</span>
</pre>
</div>
<div class="context">
<pre>1 subgoal
a : Type
H : false = true
-----------(1/1)
a
</pre>
</div>
</div>

<h2><a name="constructor"></a><code>constructor</code></h2>
<p>When your goal is to show that you can build up a term that has some type and you have a constructor to do just that, use <code>constructor</code>!</p>
<p>In this example we will prove that two is even. First we say what it means for a number to be even. We define zero to be even, and the proof of that is the term <code>even_O</code>. The next line says that if we can prove that <code>n</code> is even than we can also prove that <code>(S (S n))</code> (or <code>n + 2</code>) is even.</p>
<p>To prove our lemma, we first call <code>constructor</code>. Coq sees that our goal <span class="coq_gren">match</span>es the rightmost side of a constructor (namely <code>even_S</code>). Thus it transforms our goal into the left side of that constructor, so instead of proving that <code>(S (S O))</code> is even now we only need to prove that <code>O</code> is even. We use <code>constructor</code> again and this time Coq sees that our goal <span class="coq_gren">match</span>es the right side of a different constructor, <code>even_O</code>. This constructor has no preconditions (since zero is defined to be even, gotta start somewhere) so we are done!</p>
<div class="example">
<div class="code">
<pre><span class="checked"><span class="coq_red">Inductive</span> even : nat -&gt; <span class="coq_lblue">Prop</span>:=
 | even_O: even O
 | even_S: <span class="coq_gren">forall</span> n, even n -&gt; even (S(S n)).

<span class="coq_red">Lemma</span> <span class="coq_blue">two_is_even:</span>
  even (S (S O)).
<span class="coq_purple">Proof.</span>
  constructor.</span>
  constructor.
<span class="coq_purple">Qed.</span>
</pre>
</div>
<div class="context">
<pre>1 subgoal
-----------(1/1)
even O
</pre>
</div>
</div>

<p><strong>Use it when:</strong> your goal <span class="coq_gren">match</span>es the right side of a constructor for some type.</p>
<h2><a name="apply"></a><code>apply</code></h2>
<p>If we have a hypothesis that says that <code>x</code> implies <code>y</code>, we know that to prove <code>y</code> all we really have to do is prove <code>x</code>. We can <code>apply</code> that hypothesis to a goal of <code>y</code> to transform it into <code>x</code>.</p>
<p>In this example we prove modus ponens. We know that <code>(p -&gt; q)</code> and we want to prove <code>q</code> so we can use <code>apply</code> the hypothesis to transform the goal from <code>q</code> into <code>p</code>. Then we see that <code>p</code> is already an assumption so we are done!</p>
<div class="example">
<div class="code">
<pre><span class="checked"><span class="coq_red">Lemma</span> <span class="coq_blue">modus_ponens:</span>
  <span class="coq_gren">forall</span> p q : <span class="coq_lblue">Prop</span>, (p -&gt; q) -&gt; p -&gt; q.
<span class="coq_purple">Proof.</span>
  intros.</span>
  apply H.
  assumption.
<span class="coq_purple">Qed.</span>
</pre>
</div>
<div class="context">
<pre>1 subgoal
p : <span class="coq_lblue">Prop</span>
q : <span class="coq_lblue">Prop</span>
H : p -&gt; q
H0 : p
-----------(1/1)
q
</pre>
</div>
</div>

<p><strong>Use it when:</strong> you have a hypothesis where the conclusion (on the right of the arrow) is the same as your goal.</p>
<p><strong>Advanced usage:</strong> If we know that <code>x</code> implies <code>y</code> and we know that <code>x</code> is true, we can transform <code>x</code> into <code>y</code> in our context using <code>apply</code>.</p>
<p>In this example we prove modus ponens again. We still have our hypothesis,<br>
<code>H: p -&gt; q</code><br>
This time we <code>apply</code> it to a different hypothesis,<br>
<code>H0: p</code><br>
to turn that hypothesis into <code>q</code>.</p>
<div class="example">
<div class="code">
<pre><span class="checked"><span class="coq_red">Lemma</span> <span class="coq_blue">modus_ponens_again:</span>
  <span class="coq_gren">forall</span> p q : <span class="coq_lblue">Prop</span>, (p -&gt; q) -&gt; p -&gt; q.
<span class="coq_purple">Proof.</span>
  intros.</span>
  apply H in H0.
  assumption.
<span class="coq_purple">Qed.</span>
</pre>
</div>
<div class="context">
<pre>1 subgoal
p : <span class="coq_lblue">Prop</span>
q : <span class="coq_lblue">Prop</span>
H : p -&gt; q
H0 : p
-----------(1/1)
q
</pre>
</div>
</div>

<h2><a name="subst"></a><code>subst</code></h2>
<p>If you know that an identifier (name for something) is equal to something else, you can use <code>subst</code> to substitute the identifier for the other thing.</p>
<p>In this example we know that <code>a = b</code> and we want to show <code>b = a</code>. We can use <code>subst</code> to transform the <code>a</code> in the goal into a <code>b</code>, so our goal becomes <code>b = b</code>. Then we can finish the proof using <code>reflexivity</code>.</p>
<div class="example">
<div class="code">
<pre><span class="checked"><span class="coq_red">Inductive</span> <span class="coq_blue">bool:</span> <span class="coq_lblue">Set</span> :=
  | true
  | false.

<span class="coq_red">Lemma</span> <span class="coq_blue">equality_commutes:</span>
  <span class="coq_gren">forall</span> (a: bool) (b: bool), a = b -&gt; b = a.
<span class="coq_purple">Proof.</span>
  intros.
  subst.</span>
  reflexivity.
<span class="coq_purple">Qed.</span>
</pre>
</div>
<div class="context">
<pre>1 subgoal
a : bool
b : bool
H : a = b
-----------(1/1)
b = a
</pre>
</div>
</div>

<p><strong>Use it when:</strong> you want to transform an identifier into an equivalent term.</p>
<h2><a name="rewrite"></a><code>rewrite</code></h2>
<p>If we know two terms are equal we can transform one term into the other using <code>rewrite</code>.</p>
<p>While <code>rewrite</code> is similar to <code>subst</code>, it also works when both sides of the equality are terms. An identity is just a name like <code>x</code>, while a term can be more complex, like a function application: <code>(f x)</code>.</p>
<p>In this example we prove that if we have a function <code>f</code> and <code>(f x) = (f y)</code> then <code>(f y) = (f x)</code>. We use <code>rewrite</code> to transform <code>(f x)</code> in our goal into <code>(f y)</code> and finish the proof using <code>reflexivity</code>.</p>
<div class="example">
<div class="code">
<pre><span class="checked"><span class="coq_red">Inductive</span> <span class="coq_blue">bool:</span> <span class="coq_lblue">Set</span> :=
  | true
  | false.

<span class="coq_red">Lemma</span> <span class="coq_blue">equality_of_functions_commutes:</span>
  <span class="coq_gren">forall</span> (f: bool-&gt;bool) x y,
    (f x) = (f y) -&gt; (f y) = (f x).
<span class="coq_purple">Proof.</span>
  intros.</span>
  rewrite H.
  reflexivity.
<span class="coq_purple">Qed.</span>
</pre>
</div>
<div class="context">
<pre>1 subgoal
f : bool -&gt; bool
x : bool
y : bool
H : f x = f y
-----------(1/1)
f y = f x
</pre>
</div>
</div>

<p><strong>Use it when:</strong> you know two terms are equivalent and you want to transform one into the other.</p>
<p><strong>Advanced usage:</strong> you can also apply <code>rewrite</code> backwards, and to terms in your context.</p>
<p><strong>Backwards</strong><br>
If we have the hypothesis<br>
<code>H : f x = f y</code><br>
we can change our goal from <code>f y</code> into <code>f x</code> using <code>rewrite</code> backwards:<br>
<code>rewrite &lt;- H</code></p>
<p><strong>In context</strong><br>
We can use <code>rewrite H1 in H2</code> to transform one hypothesis using a different hypothesis.</p>
<p>In this example we prove that equality of function application is transitive. We can use either an in-context <code>rewrite</code> or a backward <code>rewrite</code> on the goal.</p>
<div class="example">
<div class="code">
<pre><span class="checked"><span class="coq_red">Inductive</span> <span class="coq_blue">bool:</span> <span class="coq_lblue">Set</span> :=
  | true
  | false.

<span class="coq_red">Lemma</span> <span class="coq_blue">equality_of_functions_transits:</span>
  <span class="coq_gren">forall</span> (f: bool-&gt;bool) x y z,
    (f x) = (f y) -&gt;
    (f y) = (f z) -&gt;
    (f x) = (f z).
<span class="coq_purple">Proof.</span>
  intros.</span>
  rewrite H0 in H. (* or rewrite &lt;- H0 *)
  assumption.
<span class="coq_purple">Qed.</span>
</pre>
</div>
<div class="context">
<pre>1 subgoal
1 subgoal
f : bool -&gt; bool
x : bool
y : bool
z : bool
H : f x = f y
H0 : f y = f z
-----------(1/1)
f x = f z
</pre>
</div>
</div>

<h2><a name="simpl"></a><code>simpl</code></h2>
<p>When we have a complex term we can use <code>simpl</code> to crunch it down.</p>
<p>In this example we prove that adding zero to any number returns the same number. We use <code>simpl</code> to "run" the <code>add</code> function in the goal. Since in the example the first argument to <code>add</code> is <code>O</code>, it simplifies the function application to just the result.</p>
<div class="example">
<div class="code">
<pre><span class="checked"><span class="coq_red">Inductive</span> nat : <span class="coq_lblue">Set</span> :=
  | O
  | S : nat -&gt; nat.

<span class="coq_red">Fixpoint</span> <span class="coq_blue">add</span> (a: nat) (b: nat) : nat :=
  <span class="coq_gren">match</span> a <span class="coq_gren">with</span>
    | O =&gt; b
    | S x =&gt; S (add x b)
  <span class="coq_gren">end</span>.

<span class="coq_red">Lemma</span> <span class="coq_blue">zero_plus_n_equals_n:</span>
  <span class="coq_gren">forall</span> n, (add O n) = n.
<span class="coq_purple">Proof.</span>
  intros.</span>
  simpl.
  reflexivity.
<span class="coq_purple">Qed.</span>
</pre>
</div>
<div class="context">
<pre>1 subgoal
n : nat
-----------(1/1)
add O n = n
</pre>
</div>
</div>

<h2><a name="cut"></a><code>cut</code></h2>
<p>Sometimes to prove a goal you need an extra hypothesis. In this case, you can add the hypothesis using <code>cut</code>. This allows you to first prove your goal using the new hypothesis, and then prove that the new hypothesis is also true.</p>
<p>In this example we will prove that if <code>x = y</code> and <code>y = z</code> then <code>f x = f z</code>, for any function <code>f</code>. This is related to transitivity. To prove the goal, we first add the intermediate proposition that <code>x = z</code>. Then we have to prove that <code>x = z</code> implies <code>f x = f z</code>, and that <code>x</code> is actually equal to <code>z</code>.</p>
<div class="example">
<div class="code">
<pre><span class="checked"><span class="coq_red">Inductive</span> <span class="coq_blue">bool:</span> <span class="coq_lblue">Set</span> :=
  | true
  | false.

<span class="coq_red">Lemma</span> <span class="coq_blue">xyz:</span>
  <span class="coq_gren">forall</span> (f: bool-&gt;bool) x y z,
    x  = y -&gt; y = z -&gt; f x = f z.
<span class="coq_purple">Proof.</span>
  intros.
  cut (x = z).</span>
  - intro. subst. reflexivity.
  - subst. reflexivity.
<span class="coq_purple">Qed.</span>
</pre>
</div>
<div class="context">
<pre>2 subgoals
f : bool -&gt; bool
x : bool
y : bool
z : bool
H : x = y
H0 : y = z
---------(1/2)
x = z -&gt; f x = f z
---------(2/2)
x = z
</pre>
</div>
</div>

<p><strong>Use it when:</strong> you want to add an intermediate hypothesis to your proof that will make the proof easier.</p>
<h2><a name="unfold"></a><code>unfold</code></h2>
<p>Sometimes you want to look inside a definition. You can use <code>unfold</code> to change the definition into its right-hand side.</p>
<div class="example">
<div class="code">
<pre><span class="checked"><span class="coq_red">Definition</span> <span class="coq_blue">inc</span> (n : nat) : nat := n + 1.

<span class="coq_red">Lemma</span> foo_defn : <span class="coq_gren">forall</span> n, inc n = S n.
<span class="coq_purple">Proof.</span>
  intros n.
  (* This doesn't work because rewrite can't "see through" the definition: *)
  Fail rewrite &lt;- plus_n_Sm.
  unfold inc.
  (* Now it works! *)
  rewrite &lt;- plus_n_Sm.
  rewrite &lt;- plus_n_O.
  reflexivity.
<span class="coq_purple">Qed.</span>
</span></pre>
</div>
</div>

<p><strong>Use it when:</strong> you want to replace a definition <span class="coq_gren">with</span> its body.</p>
<h2><a name="destruct"></a><code>destruct</code></h2>
<p>We use <code>destruct</code> to perform case analysis on a term.</p>
<p>If we have a term of some type but we don't know what the term actually is, we can use <code>destruct</code> to examine all the possible options. It generates subgoals for each possible constructor that could have been used to construct the term. Then we prove the goal for each possibility.</p>
<p>In this example we show that if we negate a boolean twice, we get the same boolean back. We cannot prove this for a general <code>b</code> but we use <code>destruct</code> to prove it for any possible value of <code>b</code> (<code>true</code> or <code>false</code>).</p>
<div class="example">
<div class="code">
<pre><span class="checked"><span class="coq_red">Inductive</span> <span class="coq_blue">bool:</span> <span class="coq_lblue">Set</span> :=
  | true
  | false.

<span class="coq_red">Definition</span> <span class="coq_blue">not</span> (b: bool) : bool :=
  <span class="coq_gren">match</span> b <span class="coq_gren">with</span>
    | true =&gt; false
    | false =&gt; true
  <span class="coq_gren">end</span>.

<span class="coq_red">Lemma</span> <span class="coq_blue">not_not_x_equals_x:</span>
  <span class="coq_gren">forall</span> b, not (not b) = b.
<span class="coq_purple">Proof.</span>
  intro.</span>
  destruct b.
  - reflexivity.
  - reflexivity.
<span class="coq_purple">Qed.</span>
</pre>
</div>
<div class="context">
<pre>1 subgoal
b : bool
-----------(1/1)
not (not b) = b
</pre>
</div>
</div>

<h2><a name="inversion"></a><code>inversion</code></h2>
<p>Sometimes you have a hypothesis that can't be true unless other things are also true. We can use <code>inversion</code> to discover other necessary conditions for a hypothesis to be true.</p>
<p>In this example we prove that if the successors of <code>a</code> and <code>b</code> are equal then <code>a</code> and <code>b</code> are also equal. We assume that <code>S a = S b</code>. However, this can only be true if <code>a = b</code> because of how we construct <code>nat</code>s. We use <code>inversion</code> to make Coq analyze the ways it can construct <code>a</code> and <code>b</code> and it realizes that they must be equal and adds it to the context.</p>
<div class="example">
<div class="code">
<pre><span class="checked"><span class="coq_red">Inductive</span> nat : <span class="coq_lblue">Set</span> :=
  | O
  | S : nat -&gt; nat.

<span class="coq_red">Lemma</span> <span class="coq_blue">successors_equal_implies_equal:</span>
  <span class="coq_gren">forall</span> a b, S a = S b -&gt; a = b.
<span class="coq_purple">Proof.</span>
  intros.</span>
  inversion H.
  reflexivity.
<span class="coq_purple">Qed.</span>
</pre>
</div>
<div class="context">
<pre>1 subgoal
a : nat
b : nat
H : S a = S b
-----------(1/1)
a = b
</pre>
</div>
</div>

<h2><a name="induction"></a><code>induction</code></h2>
<p>If we want to prove a theorem using induction, we use <code>induction</code>!</p>
<p>When we use <code>induction</code>, Coq generates subgoals for every possible constructor of the term, similar to <code>destruct</code>. However, for inductive constructors (like <code>S x</code> for <code>nat</code>s), you also get an inductive hypothesis to help you prove your goal.</p>
<p>In this example we prove that adding any number to zero gives you the same number. We perform induction on <code>n</code> and get two cases.</p>
<p>If <code>n</code> is <code>O</code> then we know that <code>(add O O)</code> is <code>O</code> so we can use reflexivity. This is the base case.</p>
<p>For the inductive case we assume that the property holds for all numbers up to <code>n</code> and we have to prove it for <code>(S n)</code> (read: <code>n+1</code>).</p>
<p>To prove this we run the <code>add</code> function for one step using <code>simpl</code>. This brings the <code>S</code> outside the <code>add</code> function and now we can <code>rewrite</code> the goal using our inductive hypothesis. Then we use <code>reflexivity</code> to finish the proof. Good ol' <code>reflexivity</code>.</p>
<div class="example">
<div class="code">
<pre><span class="checked"><span class="coq_red">Inductive</span> nat : <span class="coq_lblue">Set</span> :=
  | O
  | S : nat -&gt; nat.

<span class="coq_red">Fixpoint</span> <span class="coq_blue">add</span> (a: nat) (b: nat) : nat :=
  <span class="coq_gren">match</span> a <span class="coq_gren">with</span>
    | O =&gt; b
    | S x =&gt; S (add x b)
  <span class="coq_gren">end</span>.

<span class="coq_red">Lemma</span> <span class="coq_blue">n_plus_zero_equals_n:</span>
  <span class="coq_gren">forall</span> n, (add n O) = n.
<span class="coq_purple">Proof.</span>
  induction n.</span>
- reflexivity.
- simpl. rewrite IHn. reflexivity.
<span class="coq_purple">Qed.</span>
</pre>
</div>
<div class="context">
<pre>2 subgoals
-----------(1/2)
add O O = O
-----------(2/2)
add (S n) O = S n
</pre>
</div>
</div>

<h2><a name="auto"></a><code>auto</code></h2>
<p>Sometimes a goal looks easy but you may be feeling lazy. Why not try <code>auto</code>?</p>
<p><code>auto</code> will intro variables and hypotheses and then try applying various other tactics to solve the goal. Which other tactics does it try? Who knows man.</p>
<p>The good thing is that <code>auto</code> can't fail. At worst it will leave your goal unchanged. So go wild!</p>
<p>In this example we'll prove modus tollens using just <code>auto</code>!</p>
<div class="example">
<div class="code">
<pre><span class="checked"><span class="coq_red">Lemma</span> <span class="coq_blue">modus_tollens:</span>
<span class="coq_gren">forall</span> p q: <span class="coq_lblue">Prop</span>, (p-&gt;q) -&gt; ~q -&gt; ~p.
<span class="coq_purple">Proof.</span>
  auto.</span>
<span class="coq_purple">Qed.</span>
</pre>
</div>
<div class="context">
<pre>No more subgoals.
</pre>
</div>
</div>

<p><strong>Use it when:</strong> you think the goal is easy but you're feeling lazy.</p>
<h2><a name="intuition"></a><code>intuition</code></h2>
<p>If you thought <code>auto</code> was good, <code>intuition</code> is even better!</p>
<p>The <code>intuition</code> tactic also <code>intros</code> variables and hypotheses and applies tactics to them, including <code>auto</code>. Sometimes it works when <code>auto</code> doesn't.</p>
<p>In this example we'll prove that if we know the conjunction of <code>p</code> and <code>q</code>, we also know <code>p</code> by itself. <code>auto</code> can't solve the goal by itself but <code>intuition</code> can.</p>
<div class="example">
<div class="code">
<pre><span class="checked"><span class="coq_red">Lemma</span> <span class="coq_blue">conjunction_elimination:</span>
<span class="coq_gren">forall</span> p q, p /\ q -&gt; p.
<span class="coq_purple">Proof.</span>
  intuition.</span>
<span class="coq_purple">Qed.</span>
</pre>
</div>
<div class="context">
<pre>No more subgoals.
</pre>
</div>
</div>

<p><strong>Use it when:</strong> <code>auto</code> doesn't work but you think it should be easy to prove.</p>
<h2><a name="omega"></a><code>omega</code></h2>
<p>If you are trying to prove something "mathy" you should try the <code>omega</code> tactic. It's good at reasoning about goals involving nats and integers.</p>
<p>In this example we'll prove that an odd number can never equal an even number using  <code>omega</code>.</p>
<div class="example">
<div class="code">
<pre><span class="checked">Require Import ZArith.
(* or Require Import Omega. *)

<span class="coq_red">Lemma</span> <span class="coq_blue">odds_arent_even:</span>
<span class="coq_gren">forall</span> a b: nat, 2*a + 1 &lt;&gt; 2*b.
<span class="coq_purple">Proof.</span>
  intros.
  omega.</span>
<span class="coq_purple">Qed.</span>
</pre>
</div>
<div class="context">
<pre>No more subgoals.
</pre>
</div>
</div>

<p><strong>Use it when:</strong> your goal has some math in it.</p>
    </div>
