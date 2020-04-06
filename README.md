def get_pokemon_data():
    import requests
    stats = {}
    for ID in range(1, 152):
        url = 'https://pokeapi.co/api/v2/pokemon/{}/'.format(ID)
        response = requests.get(url)
        pokemon = response.json()
        struct = {
            'name': pokemon['name'],
            'id': pokemon['id'],
            'height': pokemon['height'],
            'weight': pokemon['weight'],
        }
        stats[ID] = struct
    return stats


def id_data(stats):
    population = []
    for i in range(1, 152):
        population.append(stats[i]['id'])
    return population


def height_data(stats):
    population = []
    for i in range(1, 152):
        population.append(stats[i]['height'])
    return population


def weight_data(stats):
    population = []
    for i in range(1, 152):
        population.append(stats[i]['weight'])
    return population


def computer_choice(data, pokemon):
    from scipy import stats
    weights = weight_data(data)
    heights = height_data(data)
    ids = id_data(data)
    weight_pct = stats.percentileofscore(weights, pokemon['weight'])
    height_pct = stats.percentileofscore(heights, pokemon['height'])
    id_pct = stats.percentileofscore(ids, pokemon['id'])
    choice = {'weight': weight_pct, 'height': height_pct, 'id': id_pct}
    return max(choice, key=lambda key: choice[key])


def random_pokemon(stats):
    import random
    pokemon_number = random.randint(1, 151)
    return stats[pokemon_number]


def play(my_stat, opponent_stat):
    if my_stat > opponent_stat:
        print('You Win!')

    elif my_stat < opponent_stat:
        print('You Lose!')

    else:
        print('Draw!')
    rematch = input('Do you want a rematch? ').lower().strip()
    if rematch == 'yes':
        return 1
    else:
        return 0


def run():
    import random

    stats = get_pokemon_data()

    rematch = 1
    while rematch:
        my_pokemon = random_pokemon(stats)
        print('You were given {}, its ID is {}, weight is {} and height is {}.'.format(my_pokemon['name'],
                                                                                       my_pokemon['id'],
                                                                                       my_pokemon['weight'],
                                                                                       my_pokemon['height']))

        opponent_pokemon = random_pokemon(stats)
        print('The opponent was given {}'.format(opponent_pokemon['name']))

        choice = input('Let\'s decide who is playing first. Heads or tails? ').lower().strip()
        if choice == 'heads':
            choice_num = 1
        elif choice == 'tails':
            choice_num = 2
        else:
            print('I did not understand your choice, try again')
            exit()

        coin = random.randint(1, 2)
        if coin == 1:
            coin_text = 'Heads'
        else:
            coin_text = 'Tails'

        if coin == choice_num:
            stat_choice = input('{}! Your turn. Which stat do you want to use? (id, height, weight) '.format(coin_text))
            newstat_choice = input('Do you want to change your stat choice? Answer yes or no ')

            if newstat_choice == 'no':
                opponent_stat = opponent_pokemon[stat_choice]
                my_stat = my_pokemon[stat_choice]
                rematch = play(my_stat, opponent_stat)
            else:
                stat_choice = input('Which stat do you want to use instead? ').lower().strip()
                my_stat = my_pokemon[stat_choice]
                opponent_stat = opponent_pokemon[stat_choice]
                rematch = play(my_stat, opponent_stat)

        else:
            print('{}! Your opponents is choosing.'.format(coin_text))
            stat_choice = computer_choice(stats, opponent_pokemon)
            print('Your opponent chose {}.'.format(stat_choice))
            my_stat = my_pokemon[stat_choice]
            opponent_stat = opponent_pokemon[stat_choice]
            rematch = play(my_stat, opponent_stat)
    exit()


run()

