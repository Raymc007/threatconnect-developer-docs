Delete Task Attributes
""""""""""""""""""""""

The code snippet below demonstrates how to delete a Task's attribute. This example assumes there is a Task with an ID of ``123456`` in the target owner. To test this code snippet, change the ``task_id`` variable to the ID of a task in your owner.

.. code-block:: python
    :emphasize-lines: 37,39-40

    # replace the line below with the standard, TC script heading described here:
    # https://docs.threatconnect.com/en/latest/python/quick_start.html#standard-script-heading
    ...

    tc = ThreatConnect(api_access_id, api_secret_key, api_default_org, api_base_url)

    # define the ID of the task we would like to retrieve
    task_id = 123456

    # create a tasks object
    tasks = tc.tasks()

    # set a filter to retrieve the task with the id: 123456
    filter1 = tasks.add_filter()
    filter1.add_id(task_id)

    try:
        # retrieve the Tasks
        tasks.retrieve()
    except RuntimeError as e:
        print('Error: {0}'.format(e))
        sys.exit(1)

    # iterate through the Tasks
    for task in tasks:
        print(task.name)

        # load the task's attributes
        task.load_attributes()

        # iterate through the task's attributes
        for attribute in task.attributes:
            print(attribute.id)

            # if the current attribute is a description attribute, delete it
            if attribute.type == 'Description':
                task.delete_attribute(attribute.id)

        # commit the changes
        task.commit()

.. note:: In the prior example, no API calls are made until the ``commit()`` method is invoked.
