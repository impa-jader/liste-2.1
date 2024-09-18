# Jader Duarte - Lista 1 - Prog 2
# Questão 1.
def Subir_escadas(steps):
    """ Seja a_n o numero de formas possiveis de subir uma escada de n degrais
    Digamos que no sua primeira ação seja subir 1 degrau, assim as formas de fazer as ações restantes serão a__(n-1)
    Analogamente, se vôce subir dois, as formas são a_(n-2)
    Note que an é a saida padrão esperada de Subir_escadas(n)
    Assim, a solução por recurção é trivial"""
    if steps ==1:
        return 1
    if steps ==2:
        return 2
    else:
        return Subir_escadas(steps-1)+Subir_escadas(steps-2)
print(Subir_escadas(10))#89

# Jader Duarte - Lista 2 - Prog 2
# Eu fiz(Questão 2)
""" 
   def __add__(self, other__vector):
        return super().__add__(other__vector)
    def __mul__(self, alpha):
        return super().__mul__(alpha)
    def __rmul__(self, f):
        return super().__rmul__(f)
    def __sub__(self,other__vector):
        return super().__add__(other__vector*(-1))
    def abs(self):
        r=0
        for i in range(self._dim):
            r+=self.coord[i]**2
        return r**1/2
        """
# Oque tá no git
class Field:
    pass

class VectorSpace:
    """VectorSpace:
    Abstract Class of vector space used to model basic linear structures
    """
    
    def __init__(self, dim: int, field: 'Field'):
        """
        Initialize a VectorSpace instance.

        Args:
            dim (int): Dimension of the vector space.
            field (Field): The field over which the vector space is defined.
        """
        self.dim = dim
        self._field = field
        
    def getField(self):
        """
        Get the field associated with this vector space.

        Returns:
            Field: The field associated with the vector space.
        """
        return self._field
    
    def getVectorSpace(self):
        """
        Get a string representation of the vector space.

        Returns:
            str: A string representing the vector space.
        """
        return f'dim = {self.dim!r}, field = {self._field!r}'
        # return self.__repr__()

    def __repr__(self):
        """
        Get a string representation of the VectorSpace instance.

        Returns:
            str: A string representing the VectorSpace instance.
        """
        # return f'dim = {self.dim!r}, field = {self._field!r}'
        return self.getVectorSpace()
    
    def __mul__(self, f):
        """
        Multiplication operation on the vector space (not implemented).

        Args:
            f: The factor for multiplication.

        Raises:
            NotImplementedError: This method is meant to be overridden by subclasses.
        """
        raise NotImplementedError
    
    def __rmul__(self, f):
        """
        Right multiplication operation on the vector space (not implemented).

        Args:
            f: The factor for multiplication.

        Returns:
            The result of multiplication.

        Note:
            This method is defined in terms of __mul__.
        """
        return self.__mul__(f)
    
    def __add__(self, v):
        """
        Addition operation on the vector space (not implemented).

        Args:
            v: The vector to be added.

        Raises:
            NotImplementedError: This method is meant to be overridden by subclasses.
        """
        raise NotImplementedError

class RealVector(VectorSpace):
    _field = float
    def __init__(self, dim, coord):
        super().__init__(dim, self._field)
        self.coord = coord
    

    @staticmethod
    def _builder(coord):
        raise NotImplementedError


    def __add__(self, other_vector):
        n_vector = []
        for c1, c2 in zip(self.coord, other_vector.coord):
            n_vector.append(c1+c2)
        return self._builder(n_vector)


    def __mul__(self, alpha):
        n_vector = []
        for c in self.coord:
            n_vector.append(alpha*c)
        return self._builder(n_vector)
    
    
    def iner_prod(self, other_vector):
        res = 0
        for c1, c2 in zip(self.coord, other_vector.coord):
            res += c1*c2
        return res


    def __str__(self):
        ls = ['[']
        for c in self.coord[:-1]:
            ls += [f'{c:2.2f}, ']
        ls += f'{self.coord[-1]:2.2f}]'
        s =  ''.join(ls)
        return s


class Vector2D(RealVector):
    _dim = 2
    def __init__(self, coord):
        if len(coord) != 2:
            raise ValueError
        super().__init__(self._dim, coord)


    @staticmethod
    def _builder(coord):
        return Vector2D(coord)
    

    def CW(self):
        return Vector2D([-self.coord[1], self.coord[0]])
    

    def CCW(self):
        return Vector2D([self.coord[1], -self.coord[0]])
    
    def __add__(self, other__vector):
        return super().__add__(other__vector)
    def __mul__(self, alpha):
        return super().__mul__(alpha)
    def __rmul__(self, f):
        return super().__rmul__(f)
    def __sub__(self,other__vector):
        return super().__add__(other__vector*(-1))
    def __abs__(self):
        r=0
        for i in range(self._dim):
            r+=self.coord[i]**2
        return r**(1/2)
#Questão 3
class Vector3D(RealVector):
    _dim = 3
    def __init__(self, coord):
        if len(coord) != 3:
            raise ValueError
        super().__init__(self._dim, coord)


    @staticmethod
    def _builder(coord):
        return Vector3D(coord)
    

    """def CW(self):
        return Vector3D([-self.coord[1], self.coord[0]])
    

    def CCW(self):
        return Vector3D([self.coord[1], -self.coord[0]])"""
    
    def __add__(self, other__vector):
        return super().__add__(other__vector)
    def __mul__(self, alpha):
        return super().__mul__(alpha)
    def __rmul__(self, f):
        return super().__rmul__(f)
    def __sub__(self,other__vector):
        return super().__add__(other__vector*(-1))
    def __abs__(self):
        r=0
        for i in range(self._dim):
            r+=self.coord[i]**2
        return r**(1/2)
   
import math
###
class Ev_polly(VectorSpace):
    _dim=math.inf
    def __init__(self,polinomio):
        self.coord= polinomio
    
    def __add__(self, pol2):
        r= self.coord.copy()
        for k in pol2.coord.keys():
            if k in r.keys():
                r[k]+= pol2.coord[k]
            else:
                r[k]= pol2.coord[k]
        return Ev_polly(r)
    
    def __mul__(self, a):
        r= self.coord.copy()
        for k in r.keys():
            r[k]=r[k]*a
        return Ev_polly(r)

    def __rmul__(self, a):
        r= self.coord.copy()
        for k in r.keys():
            r[k]=r[k]*a
        return Ev_polly(r)
    def __sub__(self, pol2):
        r= self.coord.copy()
        for k in pol2.coord.keys():
            if k in r.keys():
                r[k]+= pol2.coord[k]*(-1)
            else:
                r[k]= pol2[k]*(-1)
        return Ev_polly(r)
    def avaliar(self, x):
        r=0
        for k in self.coord.keys():
            r+=self.coord[k]*(x**k)
        return r
    def __str__(self):
        name=""
        inicio=0
        for k in self.coord.keys():
            if inicio==0:
                inicio+=1
            else:
                name+= "+"
            name+= f'{self.coord[k]}x^{k}'
        return name




if __name__ == '__main__':
    V2 = Vector2D([1, 2])
    print('V2 = ', V2)
    W2 = Vector2D([3, 4])
    print('W2 = ', W2)
    V3 = Vector3D([1, 2,15])
    print('V3 = ', V3)
    W3 = Vector3D([2, 2, 1])
    print('W3 = ', W3)

    print(V2.getVectorSpace())

    r = V2+4*W2
    abs_r= abs(W2)
    print("abs([3,4])=",abs_r)
    print('V2 + 4*W2 =', r)
    print('(V2 + 4*W2).CW() = ', r.CW())
    print('W2.CCW() = ', W2.CCW())
    print('V2.iner_prod(W2) = ', V2.iner_prod(W2))

    r = V3+4*W3
    abs_r= abs(W3)
    print("abs([2,2,1])=",abs_r)#3
    print('V3 + 4*W3 =', r)
    print('V2.iner_prod(W2) = ', V3.iner_prod(W3))

    pol1= Ev_polly({2: 3 , 3:6,  5: 1/2})
    pol2= Ev_polly({1: 3 , 2:5,  5: 1/2})
    
    print(f"f={pol1}")
    print(f"g={pol2}")
    soma_de_pol=pol1+ pol2
    print(f"({pol1})+({pol2})={soma_de_pol}") 
    prod_de_pol=2*pol1
    print(f"2({pol1})={prod_de_pol}")
    print(f"f(2)={pol1.avaliar(2)}")# 12+16+48=76 
#5 
"""import collections
import random
class FrenchDeck:
    ranks = [str(n) for n in range(2, 11)] + list('JQKA')
    suits = 'spades diamonds clubs hearts'.split()
    def __init__(self):
        self._cards = [Card(rank, suit) for suit in self.suits for rank in self.ranks]
    def __len__(self):
        return len(self._cards)
    def __getitem__(self, position):
        return self._cards[position]
    def __setitem__(self,carta,position_b):
      self._cards[position_b] = carta
myDeck = FrenchDeck()
print(myDeck[1])
random.shuffle(myDeck)"""
