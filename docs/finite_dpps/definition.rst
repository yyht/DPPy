.. _finite_dpps_definition:

Definition
**********

A finite point process :math:`\mathcal{X}` on :math:`[N] \triangleq \{1,\dots,N\}` can be understood as a random subset.
It is defined either via its:

- inclusion probabilities (also called correlation functions)

	.. math::

		\mathbb{P}[S\subset \mathcal{X}], \text{ for } S\subset [N]

- marginal probabilities

	.. math::

		\mathbb{P}[\mathcal{X}=S], \text{ for } S\subset [N]

The *determinantal* feature of DPPs stems from the fact that such inclusion, resp. marginal probabilities are given by the principal minors of the corresponding inclusion kernel :math:`\mathbf{K}` (resp. marginal kernel :math:`\mathbf{L}`).

Inclusion probabilities
=======================

We say that :math:`\mathcal{X} \sim \operatorname{DPP}(\mathbf{K})` with inclusion kernel a complex matrix :math:`\mathbf{K}` if

	.. math::
		:label: inclusion_proba

		\mathbb{P}[S\subset \mathcal{X}] = \det \mathbf{K}_S,
		\quad \forall S\subset [N]

where :math:`\mathbf{K}_S` denotes the square submatrix of :math:`\mathbf{K}` obtained by keeping only rows and columns indexed by :math:`S`.

Marginal probabilities
======================

We say that :math:`\mathcal{X} \sim \operatorname{DPP}(\mathbf{L})` with marginal kernel a complex matrix :math:`\mathbf{L}` if

	.. math::
		:label: marginal_proba

		\mathbb{P}[\mathcal{X}=S] = \frac{\det \mathbf{L}_S}{\det [I+\mathbf{L}]},
		\quad \forall S\subset [N]

Existence
=========

Some common sufficient conditions for existence are:

	.. math::
		:label: suff_cond_K

		\mathbf{K} = \mathbf{K}^{\dagger}
		\quad \text{and} \quad
		0_N \preceq \mathbf{K} \preceq I_N

where the dagger means "conjugate transpose". For the definition via marginal probabilities, sufficient conditions are

	.. math::
		:label: suff_cond_L

		\mathbf{L} = \mathbf{L}^{\dagger}
		\quad \text{and} \quad
		\mathbf{L} \succeq 0_N

.. note::

	In the following, unless otherwise specified, we work under these sufficient conditions.


.. plot:: plots/ex_plot_K_kernel.py
  :include-source:
  
.. code-block:: python

	r, N = 4, 10
	Phi = np.random.randn(r, N)
	L = Phi.T.dot(Phi)
	DPP = FiniteDPP("marginal", **{"L":L})

	print(DPP)

	# DPP defined through marginal kernel
	# Parametrized by dict_keys(['L'])
	# - sampling mode = None
	# - number of samples = 0

.. important::

	DPPs defined by an *orthogonal projection* inclusion kernel :math:`\mathbf{K}` are called *projection* DPPs.
	They are indeed valid kernels since they meet the above sufficient conditions: they are Hermitian with eigenvalues :math:`0` or :math:`1`.

	.. code-block:: python

		r, N = 4, 10
		A = np.random.randn(r, N)
		K = A.T.dot(la.inv(A.dot(A.T)).dot(A))
		proj_DPP = FiniteDPP("inclusion", projection=True, **{"K":K})
		
		print(proj_DPP)

		# DPP defined through projection inclusion kernel
		# Parametrized by dict_keys(['K'])
		# - sampling mode = None
		# - number of samples = 0


.. seealso::

	.. currentmodule:: finite_dpps

	- :class:`FiniteDPP <FiniteDPP>`
	- :cite:`KuTa12`