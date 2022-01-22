Felix Builder
=============

This Docker image serves as a counterpart to
https://github.com/acceptablesoftware/felix. While ``felix`` is used
to deploy production apps, ``felix-builder`` can be used to build them.
The issue with ``felix`` is that if you are running the container,
you do not have permissions to install additional packages with
``apk``, which can mean that dependencies for the running application
cannot be built. ``felix-builder`` solves this problem by setting up
the same environment as ``felix`` but it keeps the user as root. This
means that additional packages may be installed. A typical use case
looks like this

.. code-block:: dockerfile

    FROM ghcr.io/acceptablesoftware/felix-builder AS builder

    # Install build dependencies.
    RUN apk add gcc

    # Compile stuff.
    RUN gcc ...

    # Change user if necessary.
    USER appuser
    ENV USER=appuser
    WORKDIR /home/appuser

    # Install dependencies with pip into the user's directory
    RUN pip install black


    FROM gchr.io/acceptablesoftware/felix

    # Copy manually compiled binaries.
    COPY --from=builder /home/appuser/compiled-binary /home/appuser/

    # Copy the pip installed libraries.
    COPY --from=builder /home/appuser/.local/* /home/appuser/.local
