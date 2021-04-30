## Particle swarm optimization of optimization algorithm



####  The concept of particle swarm optimization

> Particle swarm optimization (PSO) is an evolutionary computing technology. It comes from the study of birds' predation behavior. The basic idea of particle swarm optimization algorithm is to find the optimal solution through the cooperation and information sharing among individuals
> The advantage of PSO is that it is simple and easy to implement, and there are not many parameters to adjust. It has been widely used in function optimization, neural network training, fuzzy system control and other genetic algorithms.

---

####  Analysis of particle swarm optimization

1. ***Basic ideas***

> particle swarm optimization algorithm designs a kind of massless particles to simulate the birds in the bird swarm. The particles only have two attributes: speed and position. The speed represents the speed of movement, and the position represents the direction of movement. Each particle searches for the optimal solution separately in the search space, and records it as the current individual extreme value, and shares the individual extreme value with other particles in the whole particle swarm, and finds the optimal individual extreme value as the current global optimal solution of the whole particle swarm, All particles in PSO adjust their speed and position according to the current individual extremum they find and the current global optimal solution shared by the whole PSO. The following motion chart vividly shows the process of PSO algorithm

![这里写图片描述](https://img-blog.csdn.net/20180803102329735?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2RhYWlrdWFpY2h1YW4=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

2. ***Update rules***

> PSO is initialized as a group of random particles (random solution). Then the optimal solution is found by iteration. In each iteration, particles update themselves by tracking two "extreme values" (pbest, gbest). After finding these two optimal values, the particle updates its speed and position through the following formula.

![picture](/111.png)

***

![这里写图片描述](https://img-blog.csdn.net/20180803100337670?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2RhYWlrdWFpY2h1YW4=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

```
#include <iostream>
#include <vector>
#include <cmath>
#include <map>
#include <algorithm>
#include <random>
#include <ctime>
#include <Eigen/Dense>
using namespace Eigen;
using namespace std;

const int dim = 1;
const int p_num = 10;
const int iters = 100;
const int inf = 999999;
const double pi = 3.1415;

const double v_max = 4;
const double v_min = -2;
const double pos_max = 2;
const double pos_min = -1;

vector<double> pos;
vector<double> spd;

vector<double> p_best;
double g_best;

Matrix<double, iters, p_num> f_test;
Matrix<double, iters, p_num> pos_mat;


double fun_test(double x)
{
    double res = x * x + 1;
    return res;
}


void init()
{
   
    f_test.fill(inf);
    pos_mat.fill(inf);

    static std::mt19937 rng;
    static std::uniform_real_distribution<double> distribution1(-1, 2);
    static std::uniform_real_distribution<double> distribution2(-2, 4);
    for (int i = 0; i < p_num; ++i)
    {
        pos.push_back(distribution1(rng));
        spd.push_back(distribution2(rng));
    }
    vector<double> vec;
    for (int i = 0; i < p_num; ++i)
    {
        auto temp = fun_test(pos[i]);
    
        f_test(0, i) = temp;
        pos_mat(0, i) = pos[i];
        p_best.push_back(pos[i]);
    }
    std::ptrdiff_t minRow, minCol;
    f_test.row(0).minCoeff(&minRow, &minCol);
    g_best = pos_mat(minRow, minCol);
}

void PSO()
{
    static std::mt19937 rng;
    static std::uniform_real_distribution<double> distribution(0, 1);
    for (int step = 1; step < iters; ++step)
    {
        for (int i = 0; i < p_num; ++i)
        {
         
            spd[i] = 0.5 * spd[i] + 2 * distribution(rng) * (p_best[i] - pos[i]) +
                2 * distribution(rng) * (g_best - pos[i]);
            pos[i] = pos[i] + spd[i];
      
            if (spd[i] < -2 || spd[i] > 4)
                spd[i] = 4;
            if (pos[i] < -1 || pos[i] > 2)
                pos[i] = -1;
    
            pos_mat(step, i) = pos[i];
        }
      
        for (int i = 0; i < p_num; ++i)
        {
            auto temp = fun_test(pos[i]);
            f_test(step, i) = temp;
        }
        for (int i = 0; i < p_num; ++i)
        {
            MatrixXd temp_test;
            temp_test = f_test.col(i);
            std::ptrdiff_t minRow, minCol;
            temp_test.minCoeff(&minRow, &minCol);
            p_best[i] = pos_mat(minRow, i);
        }
        g_best = *min_element(p_best.begin(), p_best.end());
    }
    cout << fun_test(g_best);
}

int main()
{
    init();
    PSO();
    system("pause");
    return 0;
}
```

| Formula 1                                                    | Formula 2                                                    | Formula 3                                                    |
| ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| ![image-20210430174217469](C:\Users\fxx\AppData\Roaming\Typora\typora-user-images\image-20210430174217469.png) | ![image-20210430174232165](C:\Users\fxx\AppData\Roaming\Typora\typora-user-images\image-20210430174232165.png) | ![image-20210430174246511](C:\Users\fxx\AppData\Roaming\Typora\typora-user-images\image-20210430174246511.png) |

~~aaaa~~

[original text]([粒子群算法及其改进算法_算法小白，嘤嘤嘤的博客-CSDN博客_改进粒子群算法](https://blog.csdn.net/weixin_45307421/article/details/94043473?ops_request_misc=%7B%22request%5Fid%22%3A%22161976705516780264093497%22%2C%22scm%22%3A%2220140713.130102334.pc%5Fall.%22%7D&request_id=161976705516780264093497&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~first_rank_v2~rank_v29-20-94043473.pc_search_result_cache&utm_term=算法))

[readme](/README.md)

