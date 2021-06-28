async def f() : fonction qui peut utiliser await
    x = await g() # met en pause f jusqu'à ce que g() renvoie une valeur. Pendant ce temps, d'autres calculs effectués.

can only await async fun