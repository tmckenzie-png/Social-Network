# Social-Network
class Person:
    def __init__(self, name):
        self.name = name
        self.friends = []

    def add_friend(self, friend):
        # Prevent duplicate friendships and self-friendships
        if friend != self and friend not in self.friends:
            self.friends.append(friend)


class SocialNetwork:
    def __init__(self):
        self.people = {}

    def add_person(self, name):
        # Check if the person already exists
        if name in self.people:
            print(f"{name} already exists in the network!")
            return

        person = Person(name)
        self.people[name] = person

    def add_friendship(self, person1_name, person2_name):
        # Make sure both people exist
        if person1_name not in self.people:
            print(f"Friendship not created. {person1_name} doesn't exist!")
            return

        if person2_name not in self.people:
            print(f"Friendship not created. {person2_name} doesn't exist!")
            return

        person1 = self.people[person1_name]
        person2 = self.people[person2_name]

        if person1 == person2:
            print("A person cannot be friends with themselves!")
            return

        # Create a bidirectional friendship
        person1.add_friend(person2)
        person2.add_friend(person1)

    def print_network(self):
        for name, person in self.people.items():
            friend_names = [friend.name for friend in person.friends]
            print(f"{name} is friends with: {', '.join(friend_names)}")


# Test the social network
if __name__ == "__main__":
    network = SocialNetwork()

    # Add people
    network.add_person("Alex")
    network.add_person("Jordan")
    network.add_person("Morgan")
    network.add_person("Taylor")
    network.add_person("Casey")
    network.add_person("Riley")

    # Test adding a duplicate person
    network.add_person("Alex")

    # Create friendships
    network.add_friendship("Alex", "Jordan")
    network.add_friendship("Alex", "Morgan")
    network.add_friendship("Jordan", "Taylor")
    network.add_friendship("Morgan", "Casey")
    network.add_friendship("Taylor", "Riley")
    network.add_friendship("Casey", "Riley")
    network.add_friendship("Morgan", "Riley")
    network.add_friendship("Alex", "Taylor")
    network.add_friendship("Jordan", "Morgan")

    # Test duplicate friendship
    network.add_friendship("Alex", "Jordan")

    # Test friendship with someone who doesn't exist
    network.add_friendship("Alex", "Johnny")

    # Print the entire network
    print("\nSocial Network:")
    network.print_network()


"""
DESIGN MEMO

A graph is the right data structure for representing a social network
because people can have connections with many different people, and
those relationships can change over time. In this project, each person
is represented as a node, and each friendship is represented as an
edge. Since friendships are bidirectional, both people are added to
each other's friend lists.

A list would not work as well because it would be harder to keep track
of which people are connected to each other. Finding specific
relationships would also require searching through the list. A tree
would be a poor choice because it organizes data in a hierarchy, where
connections generally follow parent-child relationships. Social
networks do not follow this structure because one person can be
connected to many people without having a specific ranking or order.

Using an adjacency list made the network easier to manage because
each person only stores their direct friends. Adding a friendship
requires updating two friend lists, which is efficient. Checking for
duplicate friendships requires searching through the existing friend
list, which can take longer as a person gains more friends. Printing
the network is also straightforward because the program loops through
each person and displays their connections. Overall, an adjacency
list is a good choice because it saves memory compared to an adjacency
matrix when most people are only connected to a smaller portion of
the entire network.
"""
