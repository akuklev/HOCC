Embracing Rings/rigs is a challenge for subtyping for two reasons: diamond proglem, and circularity.
- Rings extend monoids in two ways: both form a monoid with respect to both addition(+) and multiplication(·).
- We can define the class of modules over a given rig, and define (unital associative) algebras over a given rig as a monoidal object in modules over that ring.
Ultimately, we observe that a rig can be seen as an algebra over ℕ (ring as an algebra over ℤ, abelian group as a module over ℤ, etc), which should be ideally reflected by subtyping.

We propose the following syntax (inspired by Fortress) for neccesary definitions:

**Example 1**
```
@structure Rig extends Monoid(::(·)), AbMonoid(::(+))
  ...
@structure Module<R : Rig> extends AbMonoid(::(+))
  ...
@def Algebra<R : Rig>
  Monoid(::(·)) within Module<R>

@observe
  Module<ℕ>  ≡ AbMonoid(::(+))
  Rig        ≡ Monoid(::(·)) within Module<ℕ>
  Algebra<ℕ> ≡ Rig
```
