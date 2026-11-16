# Lecture2-2-谓词逻辑、推理-图集

> 主题库：人工智能　｜　源文件：Lecture2-2-谓词逻辑、推理-图集.pdf　｜　7,493 字符　｜　36 个片段　｜　更新于 2026-11-11
ArtificialIntelligence
[人工智能
ChaoYu(余超）
School of Computer Science and Engineering
[Tabl _l__

Lagicalsymbolsfixedmeaninganduse onnectivesandquantifiers =,,a Y,Y, Variables: x, x1, x2, . . . , x′, x ,  U Non-logical symbols (domain-dependent meaning and use): caritynumberofarguments Functionsymbols  arity O functions: constant symbols

Everyvariableisaterm • If t1, . .. , tn are terms and f is a function symbol of arity n, thenf(t1,...,tn)isaterm

o If t1, .. ., tn are terms and P is a predicate symbol of arity n, then P(t1,...,tn)isanatomicformula o If t1 and t2 are terms, then (t1 = t2) is an atomic formula o If α and β are formulas, and v is a variable, then ¬α, (α ∧ β), (α V β), Zv.α, ∀v.α are formulas

oUse[]and {,} o Abbreviation:(α → β)for(¬αVβ) ● Abbreviation:(α ←→ β) for (α → β) ∧(β → α) • Predicates: mixed case capitalized, e.g., Person, OlderThan Functions (and constants): mixed case uncapitalized, e.g., john,father

a Freeandboundoccurrencesofvariables 。, e.g,P(x)∧ x[P(x) √ Q(x)]  aA sentence_ formula withno free variables • Substitution: α[v/t] means α with all free occurrences of the vreplaced bytermt ln general, α[v1/t1,. .. , v /t

Aninterpretationisapair S = D,Ii • D is the domain, can be any non-empty set  • I is a mapping from the set of predicate and function symbols o If P is a predicate symbol of arity n, I(P) is an n-ary relation overD,i.e.,I(P)⊆ Dn o lfpisa O-ary predicate symbol i.e. a propositional symbol I (p ∈ { , f a s } o If f is a function symbol of arity n, I(f ) is an n-ary function over D, i.e., I(f ) : Dn → D fisa —aryfunctionsymbolieaconstantsymbol I (c)∈ D

D = {A, B, C, E} Environm ent Φ(a)= A,i Φ(b)=B, Φ(c ) = C,∂ Φ(e ) = E. Ψ(on) = {(A,B),(B,C)} Y(above)= {(A,B),(B,C),(A,C)} Y(clear)={A,E} Ψ(ontable)={C,E}

Constants:a,b,c,e Functions: No function Predicates: on:binary above:binary clear:unary ontable:unary

Termsdenoteelementsofthedomain o A variable assignment is a mapping from the set of variables tothe domainD | v||≤, = (ν) l f (t1, .. . , tn | , = (f )( t1|≥,, .. . , |tn||≥,

S, = α is read " S, satisfies α X  1 • S, = P(t1, . . . , tn) if <|t1||≥,, · . . , tn||s, i ∈ I (P)  S = = 2) f| , = |2| ,

S,= αifS,≠α S, = (α ∧ β) iff S, = α and S, = β  S, = (α √ β) iff S, = α or S, = β

{d; v} denotes a variable assignm ent just like , except that it maps u tod ● S, = v.α iff for some d ∈ D, S, {d; v} = α S, = ∀v.α iff for all d ∈ D, S, {d; v} = α Let α be a sentence. Then whether S, = α is independent of . ThuswesimplywriteS = α

D = {A, B, C, E} VX Y. on X Y)→ above X Y Φ(a)= A,Φ(b)=B, √X=A,Y=B Φ(c) = C,Φ(e) = E. √X=C, Y=A ψ(on) = {(A,B),(B,C)} √ Ψ(above) = VX Y. above(X Y)→on(X Y) {(A,B),(B,C),(A,C)} √X=A, Y=B ψ(clear)={A,E} X=A,Y=C Y(ontable)={CE}

WXaY.(clear(X V onCyXO) D={A,B,C,E} √X=A Φ(a) = A,Φ(b)= B, √ X=C,Y=B Φ(c) = C, Φ(e) = E. √ Ψ(on) = {(A,B),(B,C)} EY√X (clear(X V on(Y X) Y(above) = Y=A ? No! (×=C) {(A,B),(B,C),(A,C)} y=C? No!(X=B) y=E? No!(X=B)  Y(clear)={A,E} Y=B?No!(X=B) Y(ontable)={C,E}

o etS easetofsentences ● S = S,read Ssatisfies S, if for everyα ∈ S, S = α olf =S,wesaySisamodelofS o We say that S is satisfiable if there is S s.t. S = S, and • e.g., is {∀x(P (x) → Q (x)), P(a), ¬ Q(a)} satisfiable?

o Start with KB representing explicit beliefs, usually what the agenthasbeentoldorhaslearned 0 lmplicitbeliefs:{α| K B = α} o Actions depend on implicit beliefs, rather than explicit beliefs

KB Show that KB |= Girl FirstGrade-> Child cChild,—Male,Boy ChildaMale aBoy Kindergarten -> Child (—FirstGrade, Child) (—Kindergarten, Child) Child Female-> Girl Female   (—Child,—Female,Girl) Child  Female (Girl,—Female)  −Girl negataonof Girl  query Derivation has 9 clauses, 4new 0

● A unifier (合一项) of two formulas f and g is a substitution σ thtaat makes f and gsyntacticallyidentical ? Note that not all formulas can be unified — substitutions only affect variables. ● e.g., P(f (x), a) and P(y, f(w)) cannot be unified, as there is no way ofmakinga = f(wwitha substitution.

A substitution σ of two formulas f and g is a Most General Unifier (MGU) if aσisa unifier.  For every other unifier θ of f and g there must exist a third substitutionasuchthatθ = σa. This says that every other unifier is "more specialized" than σ. The MGU of a pair of formulas f and g is unique up to renaming.

o P ( f (x), z ) and P (y, a) ● σ = {y = f (a), x = a, z = a} is a unifier, but not an M GU ● θ = {y = f (x), z = a } is an MGU o σ = θλ,where λ = {x = a}

The MGU is the "least specialized" way of making atomic • faamulaswit eariablesmatc . WecancomputeMGUs.   Intuitively we line up the two formulas and find the first ab-expressionwheretheydisagree ? The pair of subexpressions where they first disagree is called the disagreement set. • The algorithm works by successively fixing disagreement sets until the two formulas become syntactically identical.

Given two atomic formulasfandg ① σ = { }; S = { f, g} ② If S contains an identical pair of formulas, stop and return σ as the MGUoffand g. Else find the disagreement set D = {e1, e2} of S If e1 = V a variable, and e2 =t a term not containing V (or vice-versa) then let σ = σ{V = t}; S = S{V = t}; Goto 2 Else stop,fand g cannot be unified.

P(f (a), g(x)) and P(y, y) P(a, x, h(g(z))) and P(z, h(y), h(y)) P(x, x) and P(y , f (y))

From the two clauses {ρ1} U c1 and {¬p2} U c2, where there exists a MGU σ forρl and ρ2, infer the clause (c1 U c2)σ Theorem. SF OiffSisunsatisfiable

1.(P(x), Q(g(x ))) 2. (R(a), Q(z), − P(a)) 3. R[1a,2c]{X =a} (Q(g(a)), R(a), Q(z))  "1a" means the 1st (a-th) literal in the first clause: P (x). ● "2c" means the 3rd (c-th) literal in the second clause: ¬ P(a). o 1a and 2c are the "clashing'' literals. o {X = a} is the MGU applied.

(— GradStudent(x), Student(x)) x=sue KB |= HardWorker(sue) GradStudent(sue)  KB ∀x GradStudent(x) -> Student(x) Label each step O YaStudenx HardWorker(x aantereleeant GradStudent(sue) literalsin clauses

KB = {On(a,b), On(b,c), Green(a), −Green(c)} already in CNF Query = xy[On(x,y) ∧ Green(x) ∧ —Green(y)] Note:—Q has no existentials, so yields On(x,y),—Greenx),Green(y) On(b,c cx = , = c −Green(b), Green(c)) {x = a, y = b} On(a,b) Green(c) (—Green(a),Green(b) Note: Need to use On(x,ytwice,for2 cases

Prove that yNx P(x, y) = Vx yP(x, y) ∂yNx P(x, y ) → 1.P (x, a) O - ¬∀x∂yP(x, y) ↔→ ∂x∀y¬ P(x, y) ⇒ 2.¬ P(b, y) − F R[1,2] {x = b, y = a } )   Exercises:Prove   ∀x P(x) V ∀xQ(x) = ∀x(P(x) V Q(x))  ∂x (P(x) ∧ Q(x)) = x P(x) ∧ ∂xQ(x)

(LessThan(x,y),—LessThan(succ(x),y)) KB: x=0,y=0 LessThan(succ(x),y) -> LessThan(x,y) ı LessThan(1,0) QueRy_ x=1,y=0 LessThan(O,0) —LessThan(2,0) x=2,y=0 Should fail since KB |≠ Q Infinite branchofresolvents

Theorem . SF OiffSisunsatisfiable However, there is no procedure to check if S H O, because When S is satisfiable, the search for O may not terminate

KB: Student(john) Happy(john O x[Student(x) ∧ Happy(x)]

(—Student(x), — Happy(x), A(x)) {x = john} Student(john) (—Student(john), A(john)) A(john) An answeris: John

KB Student(john) (—Student(x), —Happy(x), A(x)) {x = jane} {x = john} Query: (— Happy(jane), A(jane)) x[Student(x) > Happy(x)] (— Happy(john), A(john)) Happyjohn),Happyjane) (Happy(john), A(jane)) N ote: can have variables in answer (A(jane), A(john)) An answer is: either Jane or John

W hoever can read is literate. Dolphins are notliterate. caaFlipper is an intelligent dolphin. • Who is intelligent but cannot read. Use predicates: R(x), L(x), D(x), I (x)

[知识智能！ 推理经典逻辑推理推 （确定性推理）  [理不确定性推理 20
