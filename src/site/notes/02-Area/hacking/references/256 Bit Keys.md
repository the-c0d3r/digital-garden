---
{"dg-publish":true,"dg-path":"Garden/knowledge-base/hacking/references/256 Bit Keys.md","permalink":"/garden/knowledge-base/hacking/references/256-bit-keys/","created":"2021-01-24 16:00","updated":"2026-03-08 20:46"}
---

# 256 Bit Keys

[Does my Fast-as-Light travel method run afoul of causality or relativity?](https://worldbuilding.stackexchange.com/questions/107446/does-my-fast-as-light-travel-method-run-afoul-of-causality-or-relativity)

There's a point where "sufficiently advanced" runs into hard physical limitations. Believe it or not, the limitation on brute-forcing a [256 bit key isn't time, it's energy](https://security.stackexchange.com/a/82412/11622). (Or number of atoms in the galaxy, depending on whose math you prefer). Your system is quickly going to run into the same problem. You won’t find enough energy or matter in a solar system to run the required calculations, let alone actually do the work your system requires.

> One of the consequences of the second law of thermodynamics is that a certain amount of energy is necessary to represent information. To record a single bit by changing the state of a system requires an amount of energy no less than kT, where T is the absolute temperature of the system and k is the Boltzman constant. (Stick with me; the physics lesson is almost over.)
>
> Given that k = 1.38×10\-16 erg/K, and that the ambient temperature of the universe is 3.2 K, an ideal computer running at 3.2 K would consume 4.4×10\-16 ergs every time it set or cleared a bit. To run a computer any colder than the cosmic background radiation would require extra energy to run a heat pump.
>
> Now, the annual energy output of our Sun is about 1.21×1041 ergs. This is enough to power about 2.7×1056 single bit changes on our ideal computer; enough state changes to put a 187-bit counter through all its values. If we built a Dyson sphere around the sun and captured all its energy for 32 years, without any loss, we could power a computer to count up to 2192. Of course, it wouldn't have the energy left over to perform any useful calculations with this counter.
>
> But that's just one star, and a measly one at that. A typical supernova releases something like 1051 ergs. (About a hundred times as much energy would be released in the form of neutrinos, but let them go for now.) If all of this energy could be channeled into a single orgy of computation, a 219-bit counter could be cycled through all of its states.
>
> These numbers have nothing to do with the technology of the devices; they are the maximums that thermodynamics will allow. And they strongly imply that **brute-force attacks against 256-bit keys will be infeasible until computers are built from something other than matter and occupy something other than space**.

source: [The Doghouse: Crypteto | Schneier on Security](https://www.schneier.com/blog/archives/2009/09/the_doghouse_cr.html)