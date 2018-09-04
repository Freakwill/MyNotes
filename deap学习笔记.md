# Deap framework for GA

```python
from deap import base
from deap import creator
from deap import tools
```

## example 1: 
```python
import random

# subclass < superclass : properties
creator.create("FitnessMax", base.Fitness, weights=(1.0,))
# FitnessMax < base.Fitness: weights
creator.create("Individual", list, fitness=creator.FitnessMax)
# Individual < list: creator.FitnessMax

IND_SIZE=10

toolbox = base.Toolbox()  # toolbox of ga
toolbox.register("attr_float", random.random)
# attr_float alias-> random.random
toolbox.register("individual", tools.initRepeat, creator.Individual, toolbox.attr_float, n=IND_SIZE)
# individual shortcut-> tools.initRepeat creator.Individual:container, toolbox.attr_float:func, n=IND_SIZE
```


## pseudo-code
```python
register(alias, function, ...):
    # register a function as alias
	self.alias = pfunc = partial(function, *args, **kargs)
	pfunc.__name__ = alias
	pfunc.__doc__ = function.__doc__
```

## permutation
```python
import random

creator.create("FitnessMin", base.Fitness, weights=(-1.0,))
creator.create("Individual", list, fitness=creator.FitnessMin)

IND_SIZE=10

toolbox = base.Toolbox()
toolbox.register("indices", random.sample, range(IND_SIZE), IND_SIZE)
toolbox.register("individual", tools.initIterate, creator.Individual, toolbox.indices)
# individual -> initIterate Individual:container, indices:func
```

## example 2
```python
import array
import random

creator.create("FitnessMin", base.Fitness, weights=(-1.0,))
creator.create("Individual", array.array, typecode="d",
               fitness=creator.FitnessMin, strategy=None)
# Individual < array.array, ...
creator.create("Strategy", array.array, typecode="d")

def initES(icls, scls, size, imin, imax, smin, smax):
    # icls, scls, ... -> object of icls
    ind = icls(random.uniform(imin, imax) for _ in range(size))
    ind.strategy = scls(random.uniform(smin, smax) for _ in range(size))
    return ind

IND_SIZE = 10
MIN_VALUE, MAX_VALUE = -5., 5.
MIN_STRAT, MAX_STRAT = -1., 1. 

toolbox = base.Toolbox()
toolbox.register("individual", initES, creator.Individual,
                 creator.Strategy, IND_SIZE, MIN_VALUE, MAX_VALUE, MIN_STRAT, MAX_STRAT)
# individual -> initES, creator.Individual: icls, creator.Strategy:scls
```

## Grid
```python
toolbox.register("row", tools.initRepeat, list, toolbox.individual, n=N_COL)
toolbox.register("population", tools.initRepeat, list, toolbox.row, n=N_ROW)
```

## Seeding a Population
```python
import json

creator.create("FitnessMax", base.Fitness, weights=(1.0, 1.0))
creator.create("Individual", list, fitness=creator.FitnessMax)

def initIndividual(icls, content):
    return icls(content)

def initPopulation(pcls, ind_init, filename):
    with open(filename, "r") as pop_file:
        contents = json.load(pop_file)
    return pcls(ind_init(c) for c in contents)

toolbox = base.Toolbox()

toolbox.register("individual_guess", initIndividual, creator.Individual)
toolbox.register("population_guess", initPopulation, list, toolbox.individual_guess, "my_guess.json")

population = toolbox.population_guess()
```

## Creator
```python
def create(name, base, **kargs):
    """Creates a new class named *name* inheriting from *base* in the
    :mod:`~deap.creator` module. The new class can have attributes defined by
    the subsequent keyword arguments passed to the function create. If the
    argument is a class (without the parenthesis), the __init__ function is
    called in the initialization of an instance of the new object and the
    returned instance is added as an attribute of the class' instance.
    Otherwise, if the argument is not a class, (for example an :class:`int`),
    it is added as a "static" attribute of the class.
    
    :param name: The name of the class to create.
    :param base: A base class from which to inherit.
    :param attribute: One or more attributes to add on instanciation of this
                      class, optional.
    
    The following is used to create a class :class:`Foo` inheriting from the
    standard :class:`list` and having an attribute :attr:`bar` being an empty
    dictionary and a static attribute :attr:`spam` initialized to 1. ::
    
        create("Foo", list, bar=dict, spam=1)
        
    This above line is exactly the same as defining in the :mod:`creator`
    module something like the following. ::
    
        class Foo(list):
            spam = 1
            
            def __init__(self):
                self.bar = dict()

    The :ref:`creating-types` tutorial gives more examples of the creator
    usage.
    """
    dict_inst = {}  # class (callable), will be initialized
    dict_cls = {}   # instance, "static" attribute
    for obj_name, obj in kargs.items():
        if isinstance(obj, type):
            dict_inst[obj_name] = obj
        else:
            dict_cls[obj_name] = obj

    # Check if the base class has to be replaced
    if base in class_replacers:
        base = class_replacers[base]

    # A DeprecationWarning is raised when the object inherits from the 
    # class "object" which leave the option of passing arguments, but
    # raise a warning stating that it will eventually stop permitting
    # this option. Usually this happens when the base class does not
    # override the __init__ method from object.
    def initType(self, *args, **kargs):
        """Replace the __init__ function of the new type, in order to
        add attributes that were defined with **kargs to the instance.
        """
        for obj_name, obj in dict_inst.items():
            setattr(self, obj_name, obj())
        if base.__init__ is not object.__init__:
            base.__init__(self, *args, **kargs)

    objtype = type(str(name), (base,), dict_cls)
    objtype.__init__ = initType
    globals()[name] = objtype
```





## 源码分析

```python
def eaSimple(population, toolbox, cxpb, mutpb, ngen, stats=None,
             halloffame=None, verbose=__debug__):
    """
    :param population: A list of individuals.
    :param toolbox: A :class:`~deap.base.Toolbox` that contains the evolution
                    operators.
    :param cxpb: The probability of mating two individuals.
    :param mutpb: The probability of mutating an individual.
    :param ngen: The number of generation.
    :param stats: A :class:`~deap.tools.Statistics` object that is updated
                  inplace, optional.
    :param halloffame: A :class:`~deap.tools.HallOfFame` object that will
                       contain the best individuals, optional.
    :param verbose: Whether or not to log the statistics.
    :returns: The final population
    :returns: A class:`~deap.tools.Logbook` with the statistics of the
              evolution

    The algorithm takes in a population and evolves it in place using the
    :meth:`varAnd` method. The *cxpb* and *mutpb* arguments are passed to the
    :func:`varAnd` function.::

        evaluate(population)
        for g in range(ngen):
            population = select(population, len(population))
            offspring = varAnd(population, toolbox, cxpb, mutpb)
            evaluate(offspring)
            population = offspring

    This function expects the :meth:`toolbox.mate`, :meth:`toolbox.mutate`,
    :meth:`toolbox.select` and :meth:`toolbox.evaluate` aliases to be
    registered in the toolbox.
    """
    logbook = tools.Logbook()
    logbook.header = ['gen', 'nevals'] + (stats.fields if stats else [])

    # Evaluate the individuals with an invalid fitness
    invalid_ind = [ind for ind in population if not ind.fitness.valid]
    fitnesses = toolbox.map(toolbox.evaluate, invalid_ind)
    for ind, fit in zip(invalid_ind, fitnesses):
        ind.fitness.values = fit

    if halloffame is not None:  # update hall of fame
        halloffame.update(population)

    record = stats.compile(population) if stats else {} # statitic of population
    logbook.record(gen=0, nevals=len(invalid_ind), **record)

    # Begin the generational process
    for gen in range(1, ngen + 1):
        # Select the next generation individuals
        offspring = toolbox.select(population, len(population))

        # Vary the pool of individuals
        offspring = varAnd(offspring, toolbox, cxpb, mutpb)

        # Evaluate the individuals with an invalid fitness
        ......

        # Update the hall of fame with the generated individuals
        ......

        # Replace the current population by the offspring
        population[:] = offspring

        # Append the current generation statistics to the logbook
        record = stats.compile(population) if stats else {}
        logbook.record(gen=gen, nevals=len(invalid_ind), **record)

    return population, logbook


def varAnd(population, toolbox, cxpb, mutpb):
    """
    :param population: A list of individuals to vary.
    :param toolbox: A :class:`~deap.base.Toolbox` that contains the evolution
                    operators.
    :param cxpb: The probability of mating two individuals.
    :param mutpb: The probability of mutating an individual.
    :returns: A list of varied individuals that are independent of their
              parents.

    This variation is named *And* beceause of its propention to apply both
    crossover and mutation on the individuals. Note that both operators are
    not applied systematicaly, the resulting individuals can be generated from
    crossover only, mutation only, crossover and mutation, and reproduction
    according to the given probabilities. Both probabilities should be in
    :math:`[0, 1]`.
    """
    offspring = [toolbox.clone(ind) for ind in population]

    # Apply crossover and mutation on the offspring
    for i in range(1, len(offspring), 2):
        if random.random() < cxpb:
            offspring[i - 1], offspring[i] = toolbox.mate(offspring[i - 1],
                                                          offspring[i])
            del offspring[i - 1].fitness.values, offspring[i].fitness.values

    for i in range(len(offspring)):
        if random.random() < mutpb:
            offspring[i], = toolbox.mutate(offspring[i])
            del offspring[i].fitness.values

    return offspring
```



 The variation goes as follow:

1. the parental population $P_\mathrm{p}$ is duplicated using the `toolbox.clone` method

​    and the result is put into the offspring population $P_\mathrm{o}$. 

2. A  first loop over $P_\mathrm{o}$ is executed to mate pairs of consecutive individuals. According to the crossover probability `cxpb`, the individuals $\mathbf{x}_i$ and $\mathbf{x}_{i+1}$ are mated using the `toolbox.mate` method. The resulting children $\mathbf{y}_i$ and $\mathbf{y}_{i+1}$ replace their respective parents in $P_\mathrm{o}$. 
3. A second loop over the resulting $P_\mathrm{o}$ is executed to mutate every individual with a probability *mutpb*. When an individual is mutated it replaces its not mutated version in $P_\mathrm{o}$. The resulting $P_\mathrm{o}$ is returned.

