•	Briefly explain the work that you did on this project: What code were you given? What code did you create yourself?

  For this project, I was given some code related to the neural network as well as some basic functions to work with Treasure Maze and Game Experience classes. For my part of the code, I had to write the algorithm for the training that will be trained the network to be able to identify the path for the agent to the goal. The code that I had created is shown below:

  for epoch in range(n_epoch):
        print('\n\n\n')
        print('This is the start of the next epoch...')
        print('\n')
        # Randomly select free cell to start agent in. this is an array with length of 2
        agent_cell = np.random.randint(0, high=7, size=2)
        # Reset the game with agent at the random starting position
        qmaze.reset([0,0])
        envstate = qmaze.observe()
        loss = 0
        n_episodes = 0
        
        # while game is not over
        while qmaze.game_status() == 'not_over':
            previous_envstate = envstate
            valid_actions = qmaze.valid_actions()
            
            # Get next action
            if np.random.rand() < epsilon:
                action = random.choice(valid_actions)
            else:
                action = np.argmax(experience.predict(envstate))
            
            # Take the action
            envstate, reward, game_status = qmaze.act(action)
            # Increase episode count because taking an action makes an episode
            n_episodes += 1
            # Remember the episode
            episode = [previous_envstate, action, reward, envstate, game_status]
            experience.remember(episode)                
                
            inputs,targets = experience.get_data()            
            history = model.fit(inputs, targets, epochs=8, batch_size=24, verbose=0)
            loss = model.evaluate(inputs, targets)
            
            # if pirate agent won game
            if episode [4] == 'win':
                win_history.append(1)
                win_rate = sum(win_history) / len(win_history)
                break
            # if pirate agent lost the game
            elif episode[4] == 'lose':
                win_history.append(0)
                win_rate = sum(win_history) / len(win_history)
                break
            # else the game is not over
        
            if win_rate > epsilon:
                print('win rate is larger than epsilon!!!!')
                if completion_check(model, qmaze) == True:
                    print('completion_check() passes!!')

•	Connect your learning from throughout this course to the larger field of computer science:

  o	What do computer scientists do and why does it matter?

    Computer scientists’ job is to work with computers and software to solve any problems. Solving problems is something they can achieve by working with technology, and they help in changing the day-to-day life of people.

  o	How do I approach a problem as a computer scientist?

    The efficiency is the most important thing when it comes to solving the problem in the field of computer science. Another thing to keep in mind when approaching to solve any problem is to be ethical and responsible for any task you perform.

  o	What are my ethical responsibilities to the end user and the organization?

    As a computer scientist, we are responsible for being ethical that whatever we work for, we take responsibility of that work and give the best as we can. Always try to follow all the law and be professional.


