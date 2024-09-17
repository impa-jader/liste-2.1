# Jader Duarte - Lista 1 - Prog 2
# Questão 1.
def Subir_escadas(steps):
    """ Seja a_n o numero de formas possiveis de subir uma escada de n degrais
    Digamos que no sua primeira ação seja subir 1 degrau, assim as formas de fazer as ações restantes serão a_(n-1)
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
   def _add_(self, other_vector):
        return super()._add_(other_vector)
    def _mul_(self, alpha):
        return super()._mul_(alpha)
    def _rmul_(self, f):
        return super()._rmul_(f)
    def _sub_(self,other_vector):
        return super()._add_(other_vector*(-1))
    def abs(self):
        r=0
        for i in range(self.dim):
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
    
    def _init_(self, dim: int, field: 'Field'):
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
        # return self._repr_()

    def _repr_(self):
        """
        Get a string representation of the VectorSpace instance.

        Returns:
            str: A string representing the VectorSpace instance.
        """
        # return f'dim = {self.dim!r}, field = {self._field!r}'
        return self.getVectorSpace()
    
    def _mul_(self, f):
        """
        Multiplication operation on the vector space (not implemented).

        Args:
            f: The factor for multiplication.

        Raises:
            NotImplementedError: This method is meant to be overridden by subclasses.
        """
        raise NotImplementedError
    
    def _rmul_(self, f):
        """
        Right multiplication operation on the vector space (not implemented).

        Args:
            f: The factor for multiplication.

        Returns:
            The result of multiplication.

        Note:
            This method is defined in terms of _mul_.
        """
        return self._mul_(f)
    
    def _add_(self, v):
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
    def _init_(self, dim, coord):
        super()._init_(dim, self._field)
        self.coord = coord
    

    @staticmethod
    def _builder(coord):
        raise NotImplementedError


    def _add_(self, other_vector):
        n_vector = []
        for c1, c2 in zip(self.coord, other_vector.coord):
            n_vector.append(c1+c2)
        return self._builder(n_vector)


    def _mul_(self, alpha):
        n_vector = []
        for c in self.coord:
            n_vector.append(alpha*c)
        return self._builder(n_vector)
    
    
    def iner_prod(self, other_vector):
        res = 0
        for c1, c2 in zip(self.coord, other_vector.coord):
            res += c1*c2
        return res


    def _str_(self):
        ls = ['[']
        for c in self.coord[:-1]:
            ls += [f'{c:2.2f}, ']
        ls += f'{self.coord[-1]:2.2f}]'
        s =  ''.join(ls)
        return s


class Vector2D(RealVector):
    _dim = 2
    def _init_(self, coord):
        if len(coord) != 2:
            raise ValueError
        super()._init_(self._dim, coord)


    @staticmethod
    def _builder(coord):
        return Vector2D(coord)
# Eu fiz(Questão 2)
    def _add_(self, other_vector):
        return super()._add_(other_vector)
    def _mul_(self, alpha):
        return super()._mul_(alpha)
    def _rmul_(self, f):
        return super()._rmul_(f)
    def _sub_(self,other_vector):
        return super()._add_(other_vector*(-1))
    def abs(self):
        r=0
        for i in range(self.dim):
            r+=self.coord[i]**2
        return r**(1/2)

    def CW(self):
        return Vector2D([-self.coord[1], self.coord[0]])
    

    def CCW(self):
        return Vector2D([self.coord[1], -self.coord[0]])
   
print(__name__)
if __name__ == '__main__':
    V2 = Vector2D([1, 2])
    print('V2 = ', V2)
    W2 = Vector2D([3, 4])
    print('W2 = ', W2)


    print(V2.getVectorSpace())

    r = V2+4*W2
    print('V2 + 4*W2 =', r)
    print('(V2 + 4*W2).CW() = ', r.CW())
    print('W2.CCW() = ', W2.CCW())
    print('V2.iner_prod(W2) = ', V2.iner_prod(W2))
