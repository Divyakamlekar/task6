namespace MyTested.AspNetCore.Mvc
{
    using System;
    using Builders.Base;
    using Builders.Contracts.Base;
    using Builders.Contracts.Data;
    using Builders.Data;
    using Internal.TestContexts;
    using Utilities.Validators;

    /// <summary>
    /// Contains <see cref="Microsoft.Extensions.Caching.Memory.IMemoryCache"/> extension methods for <see cref="IBaseTestBuilderWithComponentShouldHaveTestBuilder{TBuilder}"/>.
    /// </summary>
    public static class ComponentShouldHaveTestBuilderCachingExtensions
    {
        /// <summary>
        /// Tests whether the component does not set any <see cref="Microsoft.Extensions.Caching.Memory.IMemoryCache"/> entries.
        /// </summary>
        /// <typeparam name="TBuilder">Class representing ASP.NET Core MVC test builder.</typeparam>
        /// <param name="builder">Instance of <see cref="IBaseTestBuilderWithComponentShouldHaveTestBuilder{TBuilder}"/> type.</param>
        /// <returns>The same component should have test builder.</returns>
        public static TBuilder NoMemoryCache<TBuilder>(this IBaseTestBuilderWithComponentShouldHaveTestBuilder<TBuilder> builder)
            where TBuilder : IBaseTestBuilder
        {
            var actualBuilder = (BaseTestBuilderWithComponentBuilder<TBuilder>)builder;

            if (actualBuilder.TestContext.GetMemoryCacheMock().Count > 0)
            {
                DataProviderValidator.ThrowNewDataProviderAssertionExceptionWithNoEntries(
                    actualBuilder.TestContext,
                    MemoryCacheTestBuilder.MemoryCacheName);
            }

            return actualBuilder.Builder;
        }

        /// <summary>
        /// Tests whether the component sets entries in the <see cref="Microsoft.Extensions.Caching.Memory.IMemoryCache"/>.
        /// </summary>
        /// <typeparam name="TBuilder">Class representing ASP.NET Core MVC test builder.</typeparam>
        /// <param name="builder">Instance of <see cref="IBaseTestBuilderWithComponentShouldHaveTestBuilder{TBuilder}"/> type.</param>
        /// <param name="withNumberOfEntries">Expected number of <see cref="Microsoft.Extensions.Caching.Memory.IMemoryCache"/> entries.
        /// If default null is provided, the test builder checks only if any entries are found.</param>
        /// <returns>The same component should have test builder.</returns>
        public static TBuilder MemoryCache<TBuilder>(
            this IBaseTestBuilderWithComponentShouldHaveTestBuilder<TBuilder> builder,
            int? withNumberOfEntries = null)
            where TBuilder : IBaseTestBuilder
        {
            var actualBuilder = (BaseTestBuilderWithComponentBuilder<TBuilder>)builder;

            DataProviderValidator.ValidateDataProviderNumberOfEntries(
                actualBuilder.TestContext,
                MemoryCacheTestBuilder.MemoryCacheName,
                withNumberOfEntries,
                actualBuilder.TestContext.GetMemoryCacheMock().Count);

            return actualBuilder.Builder;
        }

        /// <summary>
        /// Tests whether the component sets specific <see cref="Microsoft.Extensions.Caching.Memory.IMemoryCache"/> entries.
        /// </summary>
        /// <typeparam name="TBuilder">Class representing ASP.NET Core MVC test builder.</typeparam>
        /// <param name="builder">Instance of <see cref="IBaseTestBuilderWithComponentShouldHaveTestBuilder{TBuilder}"/> type.</param>
        /// <param name="memoryCacheTestBuilder">Builder for testing specific <see cref="Microsoft.Extensions.Caching.Memory.IMemoryCache"/> entries.</param>
        /// <returns>The same component should have test builder.</returns>
        public static TBuilder MemoryCache<TBuilder>(
            this IBaseTestBuilderWithComponentShouldHaveTestBuilder<TBuilder> builder,
            Action<IMemoryCacheTestBuilder> memoryCacheTestBuilder)
            where TBuilder : IBaseTestBuilder
        {
            var actualBuilder = (BaseTestBuilderWithComponentBuilder<TBuilder>)builder;

            memoryCacheTestBuilder(new MemoryCacheTestBuilder(actualBuilder.TestContext));

            return actualBuilder.Builder;
        }
public static class ComponentBuilderEntityFrameworkCoreExtensions
    {
        /// <summary>
        /// Sets initial values to the <see cref="Microsoft.EntityFrameworkCore.DbContext"/> on the tested component.
        /// </summary>
        /// <typeparam name="TBuilder">Class representing ASP.NET Core MVC test builder.</typeparam>
        /// <param name="builder">
        /// Instance of <see cref="IBaseTestBuilderWithComponentBuilder{TBuilder}"/> type.
        /// </param>
        /// <param name="entities">
        /// Initial values to add to the registered <see cref="Microsoft.EntityFrameworkCore.DbContext"/>.
        /// </param>
        /// <returns>The same component builder.</returns>
        public static TBuilder WithData<TBuilder>(
            this IBaseTestBuilderWithComponentBuilder<TBuilder> builder,
            IEnumerable<object> entities)
            where TBuilder : IBaseTestBuilder
            => builder
                .WithData(data => data
                    .WithEntities(entities));

        /// <summary>
        /// Sets initial values to the <see cref="Microsoft.EntityFrameworkCore.DbContext"/> on the tested component.
        /// </summary>
        /// <typeparam name="TBuilder">Class representing ASP.NET Core MVC test builder.</typeparam>
        /// <param name="builder">
        /// Instance of <see cref="IBaseTestBuilderWithComponentBuilder{TBuilder}"/> type.
        /// </param>
        /// <param name="entities">
        /// Initial values to add to the registered <see cref="Microsoft.EntityFrameworkCore.DbContext"/>.
        /// </param>
        /// <returns>The same component builder.</returns>
        public static TBuilder WithData<TBuilder>(
            this IBaseTestBuilderWithComponentBuilder<TBuilder> builder,
            params object[] entities)
            where TBuilder : IBaseTestBuilder
            => builder
                .WithData(data => data
                    .WithEntities(entities));
        
        /// <summary>
        /// Sets initial values to the <see cref="Microsoft.EntityFrameworkCore.DbContext"/> on the tested component.
        /// </summary>
        /// <typeparam name="TBuilder">Class representing ASP.NET Core MVC test builder.</typeparam>
        /// <param name="builder">
        /// Instance of <see cref="IBaseTestBuilderWithComponentBuilder{TBuilder}"/> type.
        /// </param>
        /// <param name="dbContextBuilder">
        /// Action setting the <see cref="Microsoft.EntityFrameworkCore.DbContext"/> by using <see cref="IDbContextBuilder"/>.
        /// </param>
        /// <returns>The same component builder.</returns>
        public static TBuilder WithData<TBuilder>(
            this IBaseTestBuilderWithComponentBuilder<TBuilder> builder,
            Action<IDbContextBuilder> dbContextBuilder)
            where TBuilder : IBaseTestBuilder
        {
            var actualBuilder = (BaseTestBuilderWithComponentBuilder<TBuilder>)builder;

            dbContextBuilder(new DbContextBuilder(actualBuilder.TestContext));

            return actualBuilder.Builder;
        }
    }
}

