```haskell
type Grid = Matrix Value
type Matrix a = [Row a] -- list of rows of type a
type Row a = [a] -- list of things of type a
type Value = Char

Could just be defined as type Grid = [[Char]] but intermediate types are useful


easy :: Grid
easy = ["2....1.38",
       "........5",
       ".7...6...",
       ".......13",
       ".981..257",
       "31....8..",
       "9..8...2.",
       ".5..69784",
       "4..25...."]

blank :: Grid
blank = replicate 9 (replicate 9 '.')

rows :: Matrix a -> [Row a]
rows = id

cols :: Matrix a -> [Row a]
cols = transpose -- library function, implement later to see how

boxes :: Matrix a -> [Row a]
boxes = -- black magic

-- properties of above functions 
-- rows . rows = id
-- cols . cols = id
-- boxes . boxes = id

valid :: Grid -> Bool
valid g = all nodups(rows g) &&
		  all nodups(cols g) &&
          all nodups (boxs g) 

all :: (a -> Bool) -> [a] -> Bool 
all p xs = and [p x | x <- xs] -- property p satisfied for all elements.

nodups :: Eq => [a] -> Bool 
nodups [] = True
nodups (x:xs) = not (elem x xs) && nodups xs. -- elem checks whether head is found in rest of list


--BASIC SOLVER IMPLEMENTATION
--returns list of all possible solutions for given grid
solve :: Grid -> [Grid]
solve = filter valid . collapse . choices

type Choices = [Value]
choices :: Grid -> Matrix Choices -- each element is list.
choices g = map (map choice) g 
			where
				choice v = if == '.' then ['1'..'9'] else   
				[v]
-- map applies a function to a list, so outer map applies map choice to each row, and inner map processes each cell in a row

--e.g.,
--g = ["2.",
--	  ".5"]
-- becomes
-- [[[2], ['1'..'9'],
-- ['1'..'9'], [5]]]

collapse :: Matrix [a] -> [Matrix a]
collapse m = cartesian (map cartesian m)
-- e.g.,
-- m = [[[2], ['1'..'9'],
--     ['1'..'9'], [5]]]
-- becomes
-- a list of matrices where the elements are the enumeration
-- of choices produced

--yeah idk what the fuck going on here.


cartesian :: [[a]] -> [[a]]
cartesian [] = [[]]
cartesian (xs:xss) = [y:ys | y<-xs, ys <- cartesian xss]
-- cartesian [[1,2], [3,4]] = xs = [1,2], xss = [3,4]
-- cartesian [[3,4]] = xs = [3,4] xss = []
-- cartesian [] = [[]] base case reached
-- [y:ys | y <- [3,4], ys <- cartesian []]
-- results in: [[3], [4]]
-- [y:ys | y <- [1,2], ys <- [[3], [4]]]
-- [[1,3], [1,4], [2,3], [2,4]


prune :: Matrix Choices -> Matrix Choices --prunes choices
prune = pruneBy boxes . pruneBy cols . pruneBy rows
		where pruneBy f = f . map . reduce f 
		
reduce :: Row Choices -> Row Choices
--black magic

solve2 :: filter valid . collapse . prune . choices

solve3 = filter valid . collapse . fix prune . choices

fix :: Eq a => (a -> a) -> a -> a
fix f x = if x == x' then x else fix f x' where x' =  f x

-- x = choice matrix
-- fix repeatedly applies prune to the matrix of choices until pruning process stops making changes

 
```