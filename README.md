# pcs-sqlpy-diplom

Вам предстоит разработать структуру БД для игры и предоставить несколько примеров взаимодействия с полученной БД посредством ОРМ.

## Описание игры

Вы решили создать игру, в которой будет несколько уровней. Уровни будут добавляться со временем (вами или другими разработчиками).

Каждый уровень обязательно имеет название, описание и количество баллов, которые можно будет заработать на нем (чем эффективнее игрок проходит уровень, тем больше баллов он получает).

Чтобы стимулировать игроков, вы решили добавить достижения в игру. Достижения могут быть привязаны к уровню (например, найти какой-то объект на уровне или набрать определенное количество баллов на уровне), а могут быть абстрактными (например, зайти один раз в игру или пройти все уровни). Само достижение обязательно имеет название и необязательно - описание.

Игроки после регистрации начинают проходить уровни, набирать баллы и получать достижения. Регистрация игрока означает присвоение ему уникального номера и необязательно уникального никнейма.

Конечно, игроки хотят видеть свои результаты и результаты других игроков, поэтому обязательно нужен механизм отображения доски почета: чем больше баллов суммарно по всем уровням набрал игрок - тем выше он располагается на доске почета.

## Задание

1. Продумайте структуру БД, чтобы покрывать все потребности игры.
2. Реализуйте БД и возможность управлять данными в ней
3. Продемонстрируйте возможности вашей реализации:
   1. как получить количество пройденных уровней конкретным игроком.
   2. как получить количество достижений, полученных конкретным игроком.
   3. как получить список пройденных уровней и количество полученных баллов на этих уровнях по конкретному игроку.
   4. как получить список достижений конкретного игрока.
   5. как получить доску почета вида: `игрок - его баллы`.

## Технические детали

Для реализации используйте ORM `sqlalchemy`.

В качестве БД рекомендуется использовать `Postgresql`.

### Пример подготовки данных

Важно! Это лишь пример, ваши структуры данных могут отличаться.

```python
# Create game
l1 = Level(title="level 1", description="simple level", score=30)
l2 = Level(title="level 2", description="advanced level", score=20)
l3 = Level(title="level 3", description="hard level", score=45)
l4 = Level(title="final level", description="the end", score=10)
session.add_all([l1, l2, l3, l4])
session.commit()
for level in [l1, l2, l3, l4]:
    print(level)

a1 = Achievement(title="onboarding", level=l1)
a2 = Achievement(title="secret achievement", description="I open at the end", level=l2)
a3 = Achievement(title="winner")
session.add_all([a1, a2, a3])
session.commit()
for achievement in [a1, a2, a3]:
    print(achievement)

# Register users
u1 = User(nickname="user")
u2 = User(nickname="best player")
session.add_all([u1, u2])
session.commit()
for user in [u1, u2]:
    print(user)

# Play game
session.add(LevelProgress(user=u1, level=l1, score=20))
session.add(LevelProgress(user=u2, level=l1, score=25))
session.add(LevelProgress(user=u1, level=l2, score=20))
session.add(LevelProgress(user=u2, level=l2, score=5))
session.commit()

session.add(UserAchievement(user=u1, achievement=a1))
session.add(UserAchievement(user=u2, achievement=a1))
session.add(UserAchievement(user=u1, achievement=a2))
session.commit()
```
