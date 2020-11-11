# Random

主要用于随机性选取. 该模块实现了各种分布的伪随机数生成器,  Python 使用 Mersenne Twister 作为核心生成器, 但是，因为完全确定性，它不适用于所有目的，并且完全不适合加密目的。random 模块还提供 SystemRandom 类，它使用系统函数 os.urandom() 从操作系统提供的源生成随机数。