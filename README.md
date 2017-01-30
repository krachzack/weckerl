# weckerl
*weckerl* is a small set of functions and templates for the D language that make
it a breeze to compose adaptable and robust algorithms taking vectors or
vector-like structures as input and output. Concrete implementations of vector
types can easily be changed, while the algorithms remain untouched. Heck,
algorithms can even take mixed inputs with different vector types as long as they look, feel and smell like vectors to weckerl.

Specifically, *weckerl* helps you to:
* check if a type is vector-like,
* access elements by obtaining ranges by index or swizzle.

Be sure to check out `std.algorithm` in the standard library. A lot of vector
algorithms can be implemented without much effort and rigorous testing when
relying on the well-tested and adaptable building blocks there.

## Examples
Have a look at a simple example that illustrates some of the power of *weckerl*.

This calculates the dot product of vectors of some kind, any dimension, any vector
library. The vectors might be *gl3n* or *gfm*, a simple static array or
what-have-you, yet the algorithm works the same without modification.

    import weckerl : isVec, dim, elements;
    import std.algorithm.iteration : map, sum;
    import std.range : zip;

    auto dot(S,U)(S vec1, U vec2) if(isVec!S && isVec!U && dim!S == dim!U)
    {
        // This returns successive tuples of vector elements,
        // e.g. [x1,y1] ; [x2,y2] ; [x3,y3] for two R3 vectors
        auto elementTuples = zip(vec1.elements, vec2.elements);

        // Make a range of multiplication results
        auto products = elementTuples.map!((e) => e[0] * e[1]});

        // Sum up the multiplications
        return sum(products);
    }

    // This also gives you an easy implementation of squaredLength 🌈
    auto squaredLength(S)(S vec) if(isVec!S)
    {
        return dot(vec, vec);
    }
