# Aether

Aether is a domain specific sampling language for Monte Carlo rendering. The user writes sampling code and Aether automatically generates all necessary probability density function code.

Details of the language are presented in the paper "Aether: An Embedded Domain Specific Sampling Language for Monte Carlo Rendering" (https://people.csail.mit.edu/lukea/aether) by Luke Anderson, Tzu-Mao Li, Jaakko Lehtinen, and Frédo Durand.

## A Simple Example

```cpp
#include <iostream>
#include "aether/RandomVar.h"

int main() {
  // Create a symbolic uniform random variable
  aether::variable<0> u;

  // Transform u -> sin(u)
  auto rv = make_random_var(sin(u));

  // Draw a sample from rv
  auto x = rv.Sample(0.75);

  // Compute the PDF of x using Aether's provided Pdf method
  auto pdfX = x.Pdf();

  // Compute the PDF of some other value not generated by rv
  auto pdfOther = x.Pdf(0.2);

  // x is a symbolic; x.Value() obtains the actual stored sample value
  std::cout << "x: " << x.Value() << std::endl;
  std::cout << "pdfX: " << pdfX << std::endl;
  std::cout << "pdfOther: " << pdfOther << std::endl;
  
  return 0;
}
```

### Compiling the Example

Aether is header only so doesn't need to be compiled itself before use. It has been tested with clang 3.7 (with -std=c++1z) and depends on Eigen. To compile the example:

```bash
clang++ example.cpp -std=c++1z -stdlib=libc++ -o example
```

Aether makes extensive use of template metaprogramming. To avoid template/constexpr maximum step/depth related compiler errors, more complex examples will likely require the following additional flags:

```bash
-ftemplate-depth=512
-fconstexpr-steps=127124200
-fconstexpr-depth=720
```

## Using Aether in a Renderer

Aether was designed for use in Monte Carlo rendering algorithms. [Yotsuba](https://github.com/aekul/yotsuba) is a [Mitsuba](https://github.com/mitsuba-renderer/mitsuba) based renderer that uses [Aether](https://github.com/aekul/aether) to generate samples and compute PDFs.
It contains implementations of many standard rendering algorithms and is a good place to learn about how Aether can be incorporated into a renderer and how to write rendering algorithms using Aether.

## Contact

Please contact Luke Anderson (lukea at mit.edu) or Tzu-Mao Li (tzumao at mit.edu) if there are any issues/comments/questions.
